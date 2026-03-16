This user https://www.reddit.com/user/d4nilim0n/

Keeps posting about "mokeOS." First off, I would just like to clarify I don't love just tearing down people and their creativity. However, this person has doubled down on lying so much instead of just admitting it was BS and starting over for real that I feel I should make a post informing people so they stop praising him with upvotes.

Also I am sure plenty of you are fully aware that mokeOS is BS, but obviously not everyone knows because he's still pulling in 100+ upvotes.

Here is the user's original post:

https://www.reddit.com/r/osdev/comments/1rohc5o/removed_by_moderator/

In this post, he comments using ChatGPT generated text, his post made 0 sense, he posted several images which I analysed:

My original comments from my POV only (his responses are excluded, if you'd like to read them check the linked original removed post):

Hi, I don't mean to hate the UI is nice looking, and I don't know much about the OS internals, but you're UI is web rendered which leads me to believe that this is a JavaScript mockup. I am 100% sure of this, because upon closer inspection of the images, it shows the measurements of a web rendered interface. If it were a real OS UI, it would have native UI density. In this case the padding matches Chromium padding standards 1:1. Especially things like the the glow on windows controls. That would be useless because it'd bloat the system.

Also the URL in your image shows http://apple.com but shows the Google logo, also apple.com is HTTPS not HTTP. Among other things, like having Chromium sidebars, proves that this is running in a browser environment. I saw you said it was inspired by web based OS's, but even those don't quite have these rules.

Also there is just no way you made an OS, in Python and JavaScript. It doesn't work that way.

Edit: You're code editor also is literally just plain HTML with a Monaco via CDN website. Also, even though it is heavily web made, if it were just web made as you said it would take less then 3 months to make this. It surely is not "years in development."

Andromeda isn't a browser, it has Chromium side rendering. Which means it isn't even a forked Chromium browser, but running INSIDE Chromium. And trust me, (I know a LOT about Chromium and Chrome OS) I know better about a web UI OS. And you have no idea how Chrome OS works. Chrome OS is not a HTML/CSS/JS based DE in Chrome. It IS the browser itself, and is written in C++, not web languages.

Also, no, it is NOT a proxy. You used a trick to use Google search through iframe: "iframe.src = `https://www.google.com/search?q=${encodeURIComponent(url)}&igu=1`;"

(I also said nothing about iframe, which proves more that you used iframe)

And the Google logo is a STATIC icon which is why it didn't change.

Also, graphics drivers, has NOTHING to do with this. because browser rendering on an OS needs graphics drivers? Chrome can't display shit without graphics drivers on the OS itself. Also, using Python to inject custom CSS? First of all, that's called "Stylus" and it is extremely difficult for perfect compatibility and usability for sites. Also, browsers don't allow "Python injection."

"mokeOS uses Python for the 'Kernel' services (FS, Network, Process management) and JS for the 'Shell'." You cannot use Python to make a kernel, it depends on operating systems to run CPython on top of, which uses existing C libraries from another OS. Don't lie.

These are not crappy front end thoughts lmao. It literally IS igu=1, browser layers can be seen with DOM. I can literally see igu=1. Also, cleaning a header is unstable. And no, it is not just a "FrameBuffer." You have no idea what you're saying and nor does ChatGPT. Also, some sites block custom injection scripts. What you are saying makes almost no sense.

Chromium sideloading was a reference to how you're OS is running Chromium default UI's in your screenshots lol. Sorry, but you messed up and incorrectly used FrameBuffer buzzword.

Also, you are completely ignoring the fact that you literally CANNOT build a kernel in Python without another underlying kernel with CPython. Which would mean you'd need a full OS that can handle libraries, APIs, and a package manager.

____

Up to date checks:

He only has around 278 lines of code for his entire operating system. I think we all know having a full OS especially one functioning as much as it shows in the screenshots is NOT 278 LOC.

https://github.com/littleghost-studio/host/blob/main/mokeOS/FULL%20OS%20CODE.c

⬆️This file has his entire OS source code grouped into one single file for you to read.

I ran the code through an AI detector and it came back with a 90% AI generated output. (Check screenshot for proof)

Now, if I were to assume this code was human written, it was definitely not written by this user (who claims to be 14 btw) but by this other contributor credited in the mokeOS repo who is actually credible:

https://github.com/d4nilin0n-hue/mokeOS

https://github.com/CodeAsm

https://scontent-den2-1.cdninstagram.com/v/t51.82787-19/586858714_18371985202153610_8506402218139053286_n.jpg?efg=eyJ2ZW5jb2RlX3RhZyI6InByb2ZpbGVfcGljLmRqYW5nby4xMDgwLmMyIn0&_nc_ht=scontent-den2-1.cdninstagram.com&_nc_cat=102&_nc_oc=Q6cZ2QEHuOoL-ppNGYoCrHSQJNhoSLpLcgLQCs5nsSJpt1VyQbXzR4FTdTXrC1CI0bPSv0Q3YSwoo4y99_pl0Qabl6sM&_nc_ohc=pvj2XP5wGHEQ7kNvwGwh07V&_nc_gid=MB7wqFhm-vmQhRlalG6tjg&edm=AP4sbd4BAAAA&ccb=7-5&oh=00_AfwsOUn0LicowQHbb2t0AtBwh2EpRInSMIabfC7kvPq45A&oe=69BE1DFA&_nc_sid=7a9f4b

Code analysation:

Naming Style: Function and variable names are very generic and consistently use snake_case without business-specific patterns (e.g. disable_bios_cursor, get_ram, keyboard_handler).

Comment Style: There are no comments or minimal comments, and where present, comments are generic or absent, lacking business or contextual detail.

Code Structure: The code is uniformly formatted, syntactically clean, and uses neat spacing and indentation typical of automatically generated code.

Typical AI Traits: Some helper functions appear unnecessary or overly simplified, such as into_string used to convert numbers, and consistent use of magic numbers without explanation.

Also, the lack of comments is a common sign of AI generated code, while it also cannot be, most people who use AI and don't want people to know it's slop instruct the LLM to not include any comments of code. Even hardcore developers include some comments.



Other mokeOS posts:

https://www.reddit.com/r/osdev/comments/1ruyhg0/comment/oasiipv/?context=1

https://www.reddit.com/r/osdev/comments/1rt4x2j/mokeos_progress_third_day/
https://www.reddit.com/r/osdev/comments/1rscs40/mokeos_progress_day_2/ https://www.reddit.com/r/osdev/comments/1rqfup3/mokeos_nebula_as_intended/

Also I love to see his index.html included in his workspace in vs code as well as claude:

https://github.com/littleghost-studio/host/blob/main/mokeOS/2026_03_16_0l2_Kleki.png

He also said he wasn't a newbie with C, but then later said he was. He said he'd been developing mokeOS for years, but now he claims he is on day 4 of development. He said he made a browser for his OS called Andromeda, "Microsoft Andromeda was the codename for a canceled, dual-screen mobile device and its accompanying, specialized Windows Core OS, intended to create a pocketable, notebook-like Surface device"

Claimed his kernel was in Python, only code he's shown is C.


Post URL: https://www.reddit.com/r/osdev/comments/1rvhwat/mokeos_is_fake/





