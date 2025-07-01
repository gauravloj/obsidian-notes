

- BSides 2026 CTF organize
- Crypto presentation

###############################################
#         Submitted by Miho                   #
###############################################



References to learn more about ROP:
* ROP Emporium: https://ropemporium.com/
* FuzzySecurity: https://www.fuzzysecurity.com/tutoria...
* Code Arcana: https://codearcana.com/posts/2013/05/...
* CTF101: https://ctf101.org/binary-exploitatio...
* Rapid7: https://www.rapid7.com/resources/rop-...
* Information Security Lab: cs6265/2019/tut/tut06-01-rop.html
* Ired.team: https://www.ired.team/offensive-secur...

1. read cgroups
2. read ssh tunnel/proxy
3. read linux capabilities
4. Dev website:
    1. Container security
    2. PWN shell
    3. enumeration
    4. privilege escalation
    5. Windows basics
    6. Protocols
    7. Useful links



        def get_next_nodes(r, c):
            if r > 0:
                yield r - 1, c
            if c > 0:
                yield r, c - 1
            if r < row_c - 1:
                yield r + 1, c
            if c < col_c - 1:
                yield r, c + 1

Functional prog: https://www.youtube.com/playlist?list=PLsydD1kw8jng2t2G8USQNLz0faYZetPnH


- [ ] https://csilinux.com/posts/
- [ ] https://guyinatuxedo.github.io/ - nightmare
- [ ] https://github.com/tholman?tab=repositories
- [ ] terminal trove
- [ ] https://myasiantv.ac/show/the-controllers/episode-1 - the confidence
- [ ] axing - 
    - [ ] https://mgreen27.notion.site/mgreen27/Velociraptor-DEATHcon-2023-25d9760af2ac4b419ff39c2a48f7bb2c 
    - [ ] https://youtu.be/3FNYvj2U0HM
    - [ ] https://youtu.be/sH4JCwjybGs
    - [ ] https://www.blackhillsinfosec.com/welcome-to-shark-week/
    - [ ] https://vimeo.com/showcase/wwhf2023 - CLASSIFIED
    - [ ] https://www.chiark.greenend.org.uk/doc/cpp-4.3-doc/cppinternals.html - preprocessor internals
- [ ] https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html
- [ ] https://www.technovelty.org/category/linux.html 
    - [ ] https://www.technovelty.org/linux/plt-and-got-the-key-to-code-sharing-and-dynamic-libraries.html
- [ ] https://www.brendangregg.com/USEmethod/use-linux.html
- [ ] https://docs.twistedmatrix.com/en/stable/index.html  - python event loop library
- [ ] https://cabbagepatchkids.com/
- [ ] http://geepeekay.com/gallery_os4.html - garbage pail kids
- [ ] https://suckless.org/rocks/ - software that sucks less
- [ ] https://www.youtube.com/@JakeLinux/videos - all linux
- [ ] https://www.youtube.com/@SwitchedtoLinux/playlists 
- [ ] https://www.youtube.com/@writeyourownoperatingsystem/videos - operating system in depth
- [ ] https://www.youtube.com/@pwncollege/videos - binary info
- [ ] https://pwn.college/
- [ ] https://lwn.net/Articles/276782/
- [ ] https://oooverflow.io/zero-is-you/ 
- [ ] https://soc.me/interfaces/x86-prefixes-and-escape-opcodes-flowchart 
- [ ] https://www.felixcloutier.com/x86/ - x86 instruction ref
- [ ] https://ike.mahaloz.re/1_introduction/introduction.html - system hacking
- [ ] https://ftp.gnu.org/old-gnu/Manuals/gas-2.9.1/html_chapter/as_7.html - assembler directives
- [ ] https://www.youtube.com/channel/UCo4yPkOjVM47P2toiakUOrw - i am dri brazil

—

If thou desirest with True Desire to tread the Path of Wizardly Wisdom, first learn the elementary Postures of Kernighan & Pike's The Unix Programming Environment; then, absorb the mantic puissance of March Rochkind's Advanced Unix Programming and W. Richard Stevens's Advanced Programming In The Unix Environment.
Immerse thyself, then, in the Pure Light of Maurice J. Bach's The Design Of The Unix Operating System. Neglect not the Berkelian Way; study also The Design and Implementation Of The 4.4BSD Operating System by Kirk McKusick, Keith Bostic et. al.
For useful hints, tips, and tricks, see Unix Power Tools, Tim O'Reilly, ed. Consider also the dark Wisdom to be gained from contemplation of the dread Portable C And Unix Systems Programming, e'en though it hath flowed from the keyboard of the mad and doomed Malvernite whom the world of unknowing Man misnames "J. E. Lapin".
These tomes shall instruct thy Left Brain in the Nature of the Unix System; to Feed the other half of thy Head, O Nobly Born, embrace also the Lore of its Nurture. Don Libes's and Sandy Ressler's Life With Unix will set thy Feet unerringly upon that Path; take as thy Travelling Companion the erratic but illuminating compendium called The New Hacker's Dictionary (Eric S. Raymond, ed., with Guy L. Steele Jr.).


—


———

Glider emblem:
<a href='http://www.catb.org/hacker-emblem/'>
<img src='http://www.catb.org/hacker-emblem/glider.png' alt='hacker emblem' /></a>

http://www.catb.org/~esr/hacker-emblem/glider.pic 

KDE theme: http://www.catb.org/~esr/hacker-emblem/glider-theme.tar.gz 

———


fast ram

# 3. create a super fast ram disk
mkdir -p /mnt/ram
mount -t tmpfs tmpfs /mnt/ram -o size=8192M


performance
top
vmstat
iotop
iostat
sysstat
sar
perf
strace
ltrace
ftrace


1. You can make the shell output the raw string by supplying the (q) argument: 
            % string="This is a *string* with various 'special'
            characters"
            % echo ${(q)string}
            > this\ is\ a\ \*string\*\ with\ various\ \'special\'\
            characters

2. exec N> filename : open filename for output at as Nth file descriptor

—
Links:

- [ ] https://github.com/jlevy/the-art-of-command-line
- [ ] https://perell.com/essay/50-ideas-that-changed-my-life/
- [ ] https://youtube.com/shorts/_sZ5nN9-iW0?feature=share - animes to watch
- [ ] https://trailofbits.github.io/ctf/intro/find.html 
- [ ] https://exploit.education/protostar/
- [ ] https://www.darkscience.net/faq/
- [ ] https://youtu.be/RDZnlcnmPUA - pwnie island
- [ ] https://engage.isaca.org/volunteeropportunities/about - isaca volunteer
- [ ] derbycon 
- [ ] https://www.rebootcommunications.com/event/vipss2024/ - vanQ security con
- [ ] https://github.com/JohnHammond 


Learn:

- [ ] https://github.com/infosec-au/altdns
- [ ] https://www.hacker101.com/resources#Web+hacking+tools
- [ ] seclist.org 
- [ ] https://owasp.org/www-pdf-archive/
- [ ] https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA
- [ ] https://www.youtube.com/channel/UClcE-kVhqyiHCcjYwcpfj9w - liveoverflow
- [ ] https://0xdf.gitlab.io/ - htb walkthroughs
- [ ] https://underthewire.tech/wargames - 
- [ ] https://overthewire.org/wargames/ - 
- [ ] https://danaepp.com/
- [ ] picoctf
- [ ] david bombal
- [ ] cyberdefenders
- [ ] networkchuck
- [ ] john hammond
- [ ] https://portswigger.net/
- [ ] Hacking APIs: Breaking Web Application Programming Interfaces
- [ ] The Web Application Hacker’s Handbook: Finding and Exploiting Security Flaws
- [ ] Web Application Security: Exploitation and Countermeasures for Modern Web Applications
- [ ] Bug Bounty Bootcamp: The Guide to Finding and Reporting Web Vulnerabilities
- [ ] Real-World Bug Hunting: A Field Guide to Web Hacking
- [ ] https://danaepp.com/exploit-apis-with-curl
- [ ] https://github.com/OWASP/crAPI
