
#include "arch/i386/io.h"
#include "lib/shell/shell.h"
#include "arch/i386/idt.h"
#include "lib/timer/timer.h"

void disable_bios_cursor(){
    outb(0x3D4, 0x0A);
    outb(0x3D5, 0x20);
}

void main(void* mbi){
    timer_init(1000);
    global_mbi_ptr = mbi;

    disable_bios_cursor();
    idt_init();

    init_shell();
    

    for(;;) { asm volatile("hlt"); }
}

#include "hardware.h"
#include "../../lib/string/string.h"

struct mmap_entry {
    uint32_t size;
    uint64_t addr;
    uint64_t len;
    uint32_t type;
} __attribute__((packed));

void get_ram(void* mbi_ptr, char* buffer_res){
    unsigned int flags = *(unsigned int*)mbi_ptr;
    unsigned long long total_bytes = 0;

    if(flags & (1 << 6)){
        unsigned int mmap_len  = *(unsigned int*)(mbi_ptr + 44);
        unsigned int mmap_addr = *(unsigned int*)(mbi_ptr + 48);
        struct mmap_entry* entry = (struct mmap_entry*)mmap_addr;

        while((unsigned int)entry < (mmap_addr + mmap_len)){
            if(entry->type == 1){
                total_bytes += entry->len;
            }
            entry = (struct mmap_entry*)((unsigned int)entry + entry->size + 4);
        }
    }

    unsigned int total_mb = (unsigned int)(total_bytes / 1048576);
    into_string(total_mb, buffer_res);
}

#ifndef HARDWARE_H
#define HARDWARE_H

typedef unsigned int   uint32_t;
typedef unsigned long long uint64_t;

void get_ram(void* mbi_ptr, char* buffer_res);

#endif

  #include "keyboard.h"
#include "../../lib/shell/shell.h"
#include "../screen/screen.h"
#include "../../arch/i386/io.h"

char keyboard_map[128] = {
0,27,'1','2','3','4','5','6','7','8','9','0','-','=',8,
'\t','q','w','e','r','t','y','u','i','o','p','[',']','\n',0,
'a','s','d','f','g','h','j','k','l',';','\'','`',0,
'\\','z','x','c','v','b','n','m',',','.','/',':',
'*',0,' '
};

char keyboard_buffer[256];
int buffer_idx = 0;

void keyboard_handler(){
    unsigned char scancode = inb(0x60);
    static unsigned char key_states[128];

    if(scancode & 0x80){
        key_states[scancode & 0x7F] = 0;
        outb(0x20, 0x20);
        return;
    }

    if(scancode >= 128){
        outb(0x20, 0x20);
        return;
    }

    if(key_states[scancode] == 0){
        char c = keyboard_map[scancode];
        if(c){
            if(c == 8 && screen_byte > 0){
                if(buffer_idx > 0){
                    screen_byte -= 2;
                    video_mem[screen_byte] = ' ';
                    video_mem[screen_byte + 1] = current_colour;
                    buffer_idx--;
                }             
            } else if(c == '\n'){
                keyboard_buffer[buffer_idx] = '\0';
                k_print("\n");
                if(shell_initialized == 1 && cat_commands == 0){
                    exec_command(keyboard_buffer);
                } else if(cat_commands == 1 && shell_initialized == 0){
                    exec_cat_command(keyboard_buffer);
                } else {
                    k_print("Kernel panic");
                }
                buffer_idx = 0;
            } else if(buffer_idx < 256 - 1){
                char str[2] = {c, '\0'};
                k_print(str);
                keyboard_buffer[buffer_idx] = c;
                buffer_idx++;
            }
        }
        key_states[scancode] = 1;
    }

    outb(0x20, 0x20);
}
#include "screen.h"

char *video_mem = (char*) 0xB8000;
char current_colour = 0x0F;
char custom_colour = 0x0F;
int screen_byte = 0;


void scroll(){
    for(int i = 160; i < 4000; i++){
        video_mem[i - 160] = video_mem[i];
    }

    for(int i = 3840; i < 4000; i += 2){
        video_mem[i] = ' ';
        video_mem[i + 1] = current_colour;
    }

    screen_byte = 3840;
}

void k_print(char *message){
    unsigned char *content = (unsigned char *)message;
    for(int j = 0; content[j] != '\0'; j++){
        if(screen_byte >= 4000){
            scroll();
        }
        if(content[j] == ' '){
            video_mem[screen_byte] = ' ';
            video_mem[screen_byte + 1] = current_colour;

            screen_byte+= 2;
            continue;
        }
        if(content[j] == '\n'){
            screen_byte = screen_byte / 160;
            screen_byte = screen_byte + 1;
            screen_byte = screen_byte * 160;
            
            continue;
        }
        video_mem[screen_byte] = content[j];
        screen_byte++;
        video_mem[screen_byte] = current_colour;
        screen_byte++;
    }
}

void k_print_at(char *message, int x, int y){
    int position = (y * 160) + (x * 2);
    for(int j = 0; message[j] != '\0'; j++){
        if(position >= 4000){
            scroll();
        }
        if(message[j] == ' '){
            video_mem[position] = ' ';
            video_mem[position +1] = current_colour;

            position += 2;
            continue;
        }
        if(message[j] == '\n'){
            position = position / 160;
            position = position + 1;
            position = position * 160;
            
            continue;
        }
        video_mem[position] = message[j];
        position++;
        video_mem[position] = current_colour;
        position++;
    }
}

void clean_screen(){
    for(int i = 0; i < 4000; i += 2){
        video_mem[i] = ' ';
        video_mem[i + 1] = current_colour;
    }
    screen_byte = 0;
}


void set_colour(char colour){
    if(colour == 0){
        current_colour = 0x0F;
    } else {
        current_colour = colour;
    }
}#include "idt.h"
#include "../../arch/i386/io.h"
#include "../../drivers/screen/screen.h"
#include "../../lib/timer/timer.h"

idt_entry_t idt[256];
idt_ptr_t idt_ptr;

void idt_set_gate(uint8_t num, uint32_t base, uint16_t selector, uint8_t flags){
    idt[num].base_low  = base & 0xFFFF;
    idt[num].base_high = (base >> 16) & 0xFFFF;
    idt[num].selector  = selector;
    idt[num].zero      = 0;
    idt[num].flags     = flags;
}

void pic_init(){
    outb(0x20, 0x11);
    outb(0xA0, 0x11);
    outb(0x21, 0x20);
    outb(0xA1, 0x28);
    outb(0x21, 0x04);
    outb(0xA1, 0x02);
    outb(0x21, 0x01);
    outb(0xA1, 0x01);
    outb(0x21, 0x0);
    outb(0xA1, 0xFF);
}

void idt_init(){
    idt_ptr.limit = (sizeof(idt_entry_t) * 256) - 1;
    idt_ptr.base  = (uint32_t)&idt;

    for (int i = 0; i < 256; i++) {
        idt_set_gate(i, 0, 0x08, 0x8E);
    }

    pic_init();
    idt_set_gate(33, (uint32_t)isr_keyboard, 0x08, 0x8E);
    idt_set_gate(32, (uint32_t)isr_timer, 0x08, 0x8E);

    idt_load(&idt_ptr);
}

#include "io.h"

unsigned char inb(unsigned short port) {
    unsigned char ret;
    asm volatile ("inb %1, %0" : "=a"(ret) : "Nd"(port));
    return ret;
}

void outb(unsigned short port, unsigned char val) {
    asm volatile ("outb %0, %1" : : "a"(val), "Nd"(port));
}

void outw(unsigned short port, unsigned short val) {
    asm volatile ("outw %0, %1" : : "a"(val), "Nd"(port));
}
