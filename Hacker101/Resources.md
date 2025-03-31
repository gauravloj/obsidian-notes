## Programming languages

Programming is an important part of being a successful hacker. This isn’t a comprehensive list of programming languages and nearly any can be used for most hacking tasks, especially on the web, but rather a list of languages we find especially useful or notable.

- Python and Ruby: Useful for automation and quick testing and analysis, particularly for web hacking.
- JavaScript: Can be used for the same tasks as Python and Ruby (albeit with fewer relevant libraries), but mostly useful to know for analysis of code on the web, as well as exploitation.
- Objective-C and Swift: The ability to read these will be essential if you plan to do source code review of iOS applications.
- Java and Kotlin: The ability to read these will be essential if you plan to do source code review of Android applications. Java is produced by decompilers for Android applications, which allows you to read code (roughly) equivalent to the original source, even when you only have a compiled application.
- AArch64 assembly: For advanced embedded and mobile hacking, understanding the very lowest level of abstraction is essential.

## Web hacking tools

This is a curated list of web hacking tools and is not intended to be comprehensive; rather, we want to highlight the tools we find especially useful.

- [Altdns](https://github.com/infosec-au/altdns): Altdns is a DNS recon tool that allows for the discovery of subdomains that conform to patterns. Altdns takes in words that could be present in subdomains under a domain (such as test, dev, staging), as well as a list of known subdomains.
- [Amass](https://github.com/OWASP/Amass): The OWASP Amass Project performs network mapping of attack surfaces and external asset discovery using open source information gathering and active reconnaissance techniques.
- [Aquatone](https://github.com/michenriksen/aquatone): Aquatone is a tool for visual inspection of websites across a large number of hosts, which provides a convenient overview of HTTP-based attack surface.
- [BBHT](https://github.com/nahamsec/bbht): Bug Bounty Hunting Tools is a script to install the most popular tools used while looking for vulnerabilities for a bug bounty program.
- [Burp Suite](https://portswigger.net/burp): This is the most popular proxy in web hacking circles due to its cross-platform nature and extensive featureset. See [our playlist](https://www.hacker101.com/playlists/burp_suite) to make the most of it. Also see our “Burp Suite Plugins” list for useful plugins to use.
- [chaos](https://chaos.projectdiscovery.io/): Chaos actively scans and maintains internet-wide assets’ data. This project is meant to enhance research and analyze changes around DNS for better insights.
- [Commit-stream](https://github.com/x1sec/commit-stream): Commit-stream extracts commit logs from the Github event API,  exposing the author details (name and email address) associated with Github repositories in real time.
- [Dirb](https://github.com/v0re/dirb): DIRB is a web content scanner. It launches a dictionary based attack against a web server and analyzes the response.
- [Dirsearch](https://github.com/maurosoria/dirsearch): a simple command line tool designed to brute force directories and files in websites.
- [Dngrep](https://github.com/erbbysam/DNSGrep): A utility for quickly searching presorted DNS names. Built around the Rapid7 rdns & fdns dataset.
- [Dnscan](https://github.com/rbsec/dnscan): dnscan is a python wordlist-based DNS subdomain scanner
- [Dnsgen](https://github.com/ProjectAnte/dnsgen): This tool generates a combination of domain names from the provided input. Combinations are created based on wordlist. Custom words are extracted per execution.
- [Dnsprobe](https://github.com/projectdiscovery/dnsprobe): DNSProbe is a tool built on top of retryabledns that allows you to perform multiple dns queries of your choice with a list of user supplied resolvers.
- [EyeWitnees](https://github.com/FortyNorthSecurity/EyeWitness): EyeWitness is designed to take screenshots of websites, provide some server header info, and identify any default credentials. EyeWitness is designed to run on Kali Linux. It will auto detect the file you give it with the -f flag as either being a text file with URLs on each new line, nmap xml output, or nessus xml output. The –timeout flag is completely optional, and lets you provide the max time to wait when trying to render and screenshot a web page.
- [Ffuf](https://github.com/ffuf/ffuf): A fast web fuzzer written in Go.
- [Findomain](https://github.com/Findomain/Findomain): Findomain offers a dedicated monitoring service hosted in Amazon (only the local version is free), that allows you to monitor your target domains and send alerts to Discord and Slack webhooks or Telegram chats when new subdomains are found.
- [Gau](https://github.com/lc/gau): getallurls (gau) fetches known URLs from AlienVault’s Open Threat Exchange, the Wayback Machine, and Common Crawl for any given domain. Inspired by Tomnomnom’s waybackurls.
- [gitGraber](https://github.com/hisxo/gitGraber): gitGraber is a tool developed in Python3 to monitor GitHub to search and find sensitive data in real time for different online services.
- [Httprobe](https://github.com/tomnomnom/httprobe): Takes a list of domains and probes for working http and https servers.
- [Jok3r](https://hub.docker.com/r/koutto/jok3r/): Jok3r is a framework that helps penetration testers with network infrastructure and web security assessments. Its goal is to automate as much as possible in order to quickly identify and exploit “low-hanging fruit” and “quick win” vulnerabilities on most common TCP/UDP services and most common web technologies (servers, CMS, languages…).
- [JSParser](https://github.com/nahamsec/JSParser): A python 2.7 script using Tornado and JSBeautifier to parse relative URLs from JavaScript files. This is especially useful for discovering AJAX requests when performing security research or bug bounty hunting.
- [Knockpy](https://github.com/guelfoweb/knock): Knockpy is a python tool designed to enumerate subdomains on a target domain through a word list. It is designed to scan for a DNS zone transfer and bypass the wildcard DNS record automatically, if it is enabled. Knockpy now supports queries to VirusTotal subdomains, you can set the API_KEY within the config.json file.
- [lazyrecon](https://github.com/nahamsec/lazyrecon): This is an assembled collection of tools for performing recon.
- [lazys3](https://github.com/nahamsec/lazys3): A Ruby script to brute-force for AWS s3 buckets using different permutations.
- [Masscan](https://github.com/robertdavidgraham/masscan): This is an Internet-scale port scanner. It can scan the entire Internet in under 6 minutes, transmitting 10 million packets per second, all from a single machine.
- [Massdns](https://github.com/blechschmidt/massdns): MassDNS is a simple high-performance DNS stub resolver targeting those who seek to resolve a massive amount of domain names in the order of millions or even billions. Without special configuration, MassDNS is capable of resolving over 350,000 names per second using publicly available resolvers.
- [Meg](https://github.com/tomnomnom/meg): Meg is a tool for fetching lots of URLs without taking a toll on the servers. It can be used to fetch many paths for many hosts, or fetching a single path for all hosts before moving on to the next path and repeating.
- [mitmproxy](https://mitmproxy.org/): This is an open-source proxy written in Python. Not recommended for beginners, but this can be a powerful tool.
- [Naabu](https://github.com/projectdiscovery/naabu): naabu is a port scanning tool written in Go that allows you to enumerate valid ports for hosts in a fast and reliable manner. It is a really simple tool that does fast SYN scans on the host/list of hosts and lists all ports that return a reply.
- [Nikto2](https://cirt.net/Nikto2): Like DirBuster, but also does some basic checks for known vulnerabilities.
- [Nuclei](https://github.com/projectdiscovery/nuclei): Nuclei is a fast tool for configurable targeted scanning based on templates offering massive extensibility and ease of use.
- [OWASP Zed](https://www.zaproxy.org/): OWASP Zed Attack Proxy (ZAP) is an open source tool which is offered by OWASP (Open Web Application Security Project), for penetration testing of your website/web application. It helps you find the security vulnerabilities in your application.
- [Recon_profile](https://github.com/nahamsec/recon_profile): This tool is to help create easy aliases to run via an SSH/terminal. 
- [Recon-ng](https://github.com/lanmaster53/recon-ng): Recon-ng is a full-featured reconnaissance framework designed with the goal of providing a powerful environment to conduct open source, web-based reconnaissance quickly and thoroughly.
- [Shhgit](https://github.com/eth0izzle/shhgit): Shhgit finds secrets and sensitive files across GitHub code and Gists committed in nearly real-time by listening to the GitHub Events API.
- [Shuffledns](https://github.com/projectdiscovery/shuffledns): shuffleDNS is a wrapper around massdns written in go that allows you to enumerate valid subdomains using active bruteforce, as well as resolve subdomains with wildcard handling and easy input-output support.
- [sqlmap](https://sqlmap.org/): This allows for easy discovery and exploitation of SQL injection vulnerabilities. It **will not**catch every bug or even be able to exploit some known SQLi bugs. What it will do is make your life much easier in the 80% of cases it will work for.
- [SSL Labs Server Test](https://www.ssllabs.com/ssltest/): This is an easy to use webapp for testing the SSL configuration of web servers.
- [Subfinder](https://github.com/projectdiscovery/subfinder): subfinder is a subdomain discovery tool that discovers valid subdomains for websites by using passive online sources. It has a simple modular architecture and is optimized for speed. subfinder is built for doing one thing only - passive subdomain enumeration, and it does that very well.
- [Subjack](https://github.com/haccer/subjack): Subjack is a Subdomain Takeover tool written in Go designed to scan a list of subdomains concurrently and identify ones that are able to be hijacked. With Go’s speed and efficiency, this tool really stands out when it comes to mass-testing. Always double check the results manually to rule out false positives.
- [Sublert](https://github.com/yassineaboukir/sublert): Sublert is a security and reconnaissance tool that was written in Python to leverage certificate transparency for the sole purpose of monitoring new subdomains deployed by specific organizations and an issued TLS/SSL certificate. The tool is supposed to be scheduled to run periodically at fixed times, dates, or intervals (Ideally each day). New identified subdomains will be sent to Slack workspace with a notification push. Furthermore, the tool performs DNS resolution to determine working subdomains.
- [Sublist3r](https://github.com/aboul3la/Sublist3r): Sublist3r is a python tool designed to enumerate subdomains of websites using OSINT. It helps penetration testers and bug hunters collect and gather subdomains for the domain they are targeting. Sublist3r enumerates subdomains using many search engines such as Google, Yahoo, Bing, Baidu and Ask. Sublist3r also enumerates subdomains using Netcraft, Virustotal, ThreatCrowd, DNSdumpster and ReverseDNS.
- [Teh_s3_bucketeers](https://github.com/tomdev/teh_s3_bucketeers): Teh_s3_bucketeers is a security tool to discover S3 buckets on Amazon’s AWS platform.
- [Unfurl](https://github.com/JLospinoso/unfurl): Unfurl is a tool that analyzes large collections of URLs and estimates their entropies to sift out URLs that might be vulnerable to attack.
- [Virtual-host-discovery](https://github.com/jobertabma/virtual-host-discovery): This is a basic HTTP scanner that enumerates virtual hosts on a given IP address. During recon, this might help expand the target by detecting old or deprecated code. It may also reveal hidden hosts that are statically mapped in the developer’s /etc/hosts file.
- [Waybackurls](https://github.com/tomnomnom/waybackurls): Accept line-delimited domains on stdin, fetch known URLs from the Wayback Machine for *.domain and output them on stdout.
- [Webscreenshot](https://github.com/maaaaz/webscreenshot): A simple script to screenshot a list of websites, based on the url-to-image PhantomJS script.
- [Wfuzz](https://github.com/xmendez/wfuzz): Wfuzz has been created to facilitate the task in web applications assessments and it is based on a simple concept: it replaces any reference to the FUZZ keyword by the value of a given payload.
- [Whatweb](https://github.com/urbanadventurer/WhatWeb): WhatWeb recognises web technologies including content management systems (CMS), blogging platforms, statistic/analytics packages, JavaScript libraries, web servers, and embedded devices. WhatWeb has over 1800 plugins, each to recognise something different. WhatWeb also identifies version numbers, email addresses, account IDs, web framework modules, SQL errors, and more.
- [Wpscan](https://github.com/wpscanteam/wpscan): WPScan is a free (for non-commercial use) black box WordPress security scanner written for security professionals and bloggers to test the security of their sites.
- [nmmapper](https://www.nmmapper.com/sys/tools/subdomainfinder/): A Collection of 8 subdomain finders hosted online for quick usage, this include, sublist3r, amass, findomain,knocky,anubis subdomain finder, dnscan, nmap subrute,lepu subdomain and even waybackurl.

## Burp Suite plugins

[Burp Suite](https://portswigger.net/burp): The quintessential web app hacking tool. Once you hit 500 reputation on HackerOne, you are eligible for a free 3-month license of Burp Suite Pro!

This is a curated list of Burp plugins and is not intended to be comprehensive; rather, we want to highlight the plugins we find especially useful.

- [ActiveScan++](https://portswigger.net/bappstore/3123d5b5f25c4128894d97ea1acc4976): ActiveScan++ extends Burp Suite’s active and passive scanning capabilities. Designed to add minimal network overhead, it identifies application behavior that may be of interest to advanced testers.
- [Autorepeater Burp](https://github.com/nccgroup/AutoRepeater): Automated HTTP request repeating with Burp Suite.
- [Autorize Burp](https://portswigger.net/bappstore/f9bbac8c4acf4aefa4d7dc92a991af2f): Autorize is an extension aimed at helping the penetration tester to detect authorization vulnerabilities —one of the more time-consuming tasks in a web application penetration test.
- [BurpSentinel](https://github.com/dobin/BurpSentinel): With BurpSentinel it is possible for the penetration tester to quickly and easily send a lot of malicious requests to parameters of a HTTP request. Not only that, but it also shows a lot of information of the HTTP responses, corresponding to the attack requests. It’s easy to find low-hanging fruit and hidden vulnerabilities like this, and it also allows the tester to focus on more important stuff!
- [Flow](https://portswigger.net/bappstore/ee1c45f4cc084304b2af4b7e92c0a49d): This extension provides a Proxy history-like view along with search filter capabilities for all Burp tools.
- [Headless Burp](https://portswigger.net/bappstore/d54b11f7af3c4dfeb6b81fb5db72e381): This extension allows you to run Burp Suite’s Spider and Scanner tools in headless mode via the command-line.
- [Logger++](https://portswigger.net/bappstore/470b7057b86f41c396a97903377f3d81): Logger++ is a multi-threaded logging extension for Burp Suite. In addition to logging requests and responses from all Burp Suite tools, the extension allows advanced filters to be defined to highlight interesting entries or filter logs to only those which match the filter.
- [WSDL Wizard](https://portswigger.net/bappstore/ef2f3f1a593d417987bb2ddded760aee): This extension scans a target server for WSDL files. After performing normal mapping of an application’s content, right click on the relevant target in the site map, and choose “Scan for WSDL files” from the context menu. The extension will search the already discovered contents for URLs with the .wsdl file extension, and guess the locations of any additional WSDL files based on the file names known to be in use. The results of the scanning appear within the extension’s output tab in the Burp Extender tool.

## Mobile hacking tools

In order to get hands-on experience while reading this book, you need the following software. Download links and installation steps are shown later in the book.

- [Android Studio](https://developer.android.com/sdk/index.html)
- An Android emulator
- Burpsuite
- [Apktool])https://apktool.org/
- [Dex2jar](http://sourceforge.net/projects/dex2jar/), https://github.com/pxb1988/dex2jar
- [JD-GUI](http://jd.benow.ca/)
- [Drozer](https://labs.withsecure.com/tools/drozer)
- Vulnerable Apps:
	- [GoatDroid App](https://github.com/jackMannino/OWASP-GoatDroid-Project)
	- [SSH Droid](https://play.google.com/store/apps/details?id=berserker.android.apps.sshdroid&hl=en)
	- [FTP server](https://play.google.com/store/apps/details?id=com.theolivetree.ftpserver&hl=en)
- [QARK](https://github.com/linkedin/qark/)
- Cydia Substrate
- [Introspy Android](https://github.com/iSECPartners/Introspy-Android)
- [Introspy Analyzer](https://github.com/iSECPartners/Introspy-Analyzer)
- Xposed Framework
- [Frida](https://github.com/frida/frida), http://www.frida.re/docs/android/

This is a curated list of mobile hacking tools and is not intended to be comprehensive; rather, we want to highlight the tools we find especially useful.


- [dex2jar](https://github.com/pxb1988/dex2jar): Converts dex code (Android bytecode) into Java JAR files for manipulation or decompilation.
- [dotPeek](https://www.jetbrains.com/decompiler/): A .NET decompiler, for use with Xamarin Android applications.
- [Frida “Universal” SSL Unpinner](https://gist.github.com/teknogeek/4dc35fb3801bd7f13e5f0da5b784c725): Universal unpinner.
- [Frida](https://frida.re/): This is an instrumentation system allowing injection of JavaScript or native libraries into arbitrary mobile applications on iOS and Android. In essence, it makes it painless to change, enhance, or disable functionality in applications.
- [Genymotion](https://www.genymotion.com/): Cross-platform Android emulator for developers & QA engineers. Develop & automate your tests to deliver best quality apps.
- [Jadx](https://github.com/skylot/jadx): Jadx is a dex to Java decompiler. The command line and GUI tools for producing Java source code from Android Dex and Apk files.
- [JD-GUI](https://java-decompiler.github.io/): This is a Java decompiler, useful after dex2jar for easy analysis of Android apps.
- [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF): An automated, all-in-one mobile application (Android/iOS/Windows) pen-testing, malware analysis and security assessment framework capable of performing static and dynamic analysis.
- [Online Decompiler](https://www.decompiler.com/): APK, JAR and .NET online decompiler.
- [Radare2](https://rada.re/n/): A free/libre toolchain for easing several low level tasks, such as forensics, software reverse engineering, exploiting, debugging, etc. It is composed by a large number of libraries (which are extended with plugins) and programs that can be automated with almost any programming language.

## Android hacking tools

This is a curated list of Android hacking tools and is not intended to be comprehensive; rather, we want to highlight the tools we find especially useful.

### Videos

- [Hacker101 - Android Quickstart](https://www.youtube.com/watch?v=y0O3sCX9ftM)
- [Hacker101 - Mobile Hacking Crash Course](https://www.youtube.com/watch?v=hKF89TXttnw)
- [Hacker101: Common Android Bugs Pt. 1](https://www.youtube.com/watch?v=sQ_34dI_geU)
- [Hacker101: Common Android Bugs Pt. 2](https://www.youtube.com/watch?v=tt1f4pcI0jo)
- [Android Pentesting Video Playlist](https://www.youtube.com/playlist?list=PLgnrksnL_Rn09gGTTLgi-FL7HxPOoDk3R)
- [Low Competition Bug Hunting (What to Learn) - ft. #AndroidHackingMonth](https://www.youtube.com/watch?v=Pkd_j31Jtfc)

### Blog Posts

- [#Androidhackingmonth: Introduction to Android Hacking by @0xteknogeek](https://www.hackerone.com/blog/androidhackingmonth-intro-to-android-hacking)
- [QA with Android Hacker: Bagipro](https://www.hackerone.com/blog/AndroidHackingMonth-qa-with-bagipro)
- [Hacking SMS API Service Provider of a Company - Android App Static Security Analysis](https://blog.securitybreached.org/2020/02/19/hacking-sms-api-service-provider-of-a-company-android-app-static-security-analysis-bug-bounty-poc/)
- [Getting Started in Android Apps Pen-testing (Part-1)](https://blog.securitybreached.org/2020/03/17/getting-started-in-android-apps-pentesting/)

### Example Reports

- [Periscope android app deeplink leads to CSRF in follow action](https://hackerone.com/reports/583987)
- [Twitter lite(Android): Vulnerable to local file steal, Javascript injection, Open redirect](https://hackerone.com/reports/499348)
- [Golden techniques to bypass host validations in Android apps](https://hackerone.com/reports/431002)
- [SQL Injection found in NextCloud Android App Content Provider](https://hackerone.com/reports/291764)
- [Quora Android - Possible to steal arbitrary files from mobile device](https://hackerone.com/reports/258460)
- [Opening arbitrary URLs/XSS in SAMLAuthActivity](https://hackerone.com/reports/283058)
- [Access of Android protected components via embedded intent](https://hackerone.com/reports/200427)

### Other Resources

- [The Mobile Hacking CheatSheet](https://github.com/randorisec/MobileHackingCheatSheet)
- [Mobile App Pentest Cheatsheet](https://github.com/tanprathan/MobileApp-Pentest-Cheatsheet)
- [Awesome Mobile Security](https://github.com/vaib25vicky/awesome-mobile-security)
- [Mobile Penetration Testing Kit](https://www.eshlomo.us/mobile-penetration-testing-kit/)
- [Periscope android app deeplink leads to CSRF in follow action](https://hackerone.com/reports/583987)

### Practice Labs

- [Damn Insecure and vulnerable App for Android](https://github.com/payatu/diva-android)
- [InjuredAndroid](https://github.com/B3nac/InjuredAndroid)
- [Android-InsecureBankv2](https://github.com/dineshshetty/Android-InsecureBankv2)

## Desktop / embedded hacking tools

This is a curated list of hacking tools for native applications and embedded devices and is not intended to be comprehensive; rather, we want to highlight the tools we find especially useful.

- [american fuzzy lop](https://lcamtuf.coredump.cx/afl/): AFL is an extremely powerful fuzzer, enabling detection of complicated bugs in many applications and libraries.
- [Binary Ninja](https://binary.ninja/): Another low-cost alternative to IDA. Its API is perhaps the most powerful of the three for automating analysis of code.
- [Binwalk](https://github.com/ReFirmLabs/binwalk): Used for firmware analysis and extraction. This is primarily useful for embedded Linux devices.
- [dotPeek](https://www.jetbrains.com/decompiler/): A powerful decompiler for .NET assemblies.
- [GNU strings](https://en.wikipedia.org/wiki/Strings_(Unix)): Finds strings in arbitrary binaries. While not strictly for reverse-engineering, it is among the most useful tools around.
- [Hopper](https://www.hopperapp.com/): This is a fantastic, low-cost disassembler and decompiler that runs on macOS and Linux. While it’s no replacement for IDA, it is a great choice for most applications.
- [HxD](https://mh-nexus.de/en/hxd/) (Windows) [0xED](https://www.suavetech.com/0xed/) (macOS): These are graphical hex editors, useful for analysis and manipulation of files and block devices.
- [IDA Pro and Hex-Rays Decompiler](https://hex-rays.com/ida-pro/): IDA is the absolute gold standard for disassemblers and its decompiler plugins are the gold standard for decompilation. It is a wonderful tool with support for nearly every obscure platform and an extensive (if confusing) SDK to add nearly any feature you can imagine. However, its price makes it difficult to justify.
- [PE Explorer](http://www.heaventools.com/overview.htm): This is a great tool for analyzing the PE binaries used on Windows. It allows for exploration of the structures of the executable itself, as well as resources.
- [PEiD](https://www.aldeid.com/wiki/PEiD): Tool for detecting cryptors, packers, and encryption routines in Windows PE binaries.
- [QEMU](https://www.qemu.org/): An emulator and virtual machine supporting a large number of systems/architectures. This makes it useful for things like running embedded firmware, but also includes [debugging facilities](https://en.wikibooks.org/wiki/QEMU/Debugging_with_QEMU) that make it an optimal tool for hacking. Can be combined with AFL for fuzzing of binaries that aren’t for your native architecture.
- [Radare2](https://rada.re/n/radare2.html): This is a set of tools for doing analysis of binaries. It includes everything from disassembly to debugging and more.
- [Unicorn Engine](https://www.unicorn-engine.org/): This is a library rather than a standalone tool, but it makes writing quick emulators a breeze. Particularly useful for reverse-engineering.

## Exploitation resources

This is a curated list of exploitation resources and is not intended to be comprehensive; rather, we want to highlight the tools we find especially useful.

- [NoSQLMap](https://github.com/codingo/NoSQLMap): NoSQLMap is an open source Python tool designed to audit for, as well as automate injection attacks, and exploit default configuration weaknesses in NoSQL databases and web applications using NoSQL to disclose or clone data from the database.
- [Retire.JS](https://addons.mozilla.org/en-US/firefox/addon/retire-js/): Scanning website for vulnerable js libraries.
- [Spiderfoot](https://github.com/smicallef/spiderfoot): SpiderFoot is an open source intelligence (OSINT) automation tool. It integrates with just about every data source available, and automates OSINT collection so that you can focus on data analysis.
- [sqlmap](https://sqlmap.org/): sqlmap is an open source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws and taking over database servers. It comes with a powerful detection engine, many niche features for the ultimate penetration tester, and a broad range of switches including: database fingerprinting, over data fetching from the database, accessing the underlying file system, and executing commands on the operating system via out-of-band connections.
- [SQLNinja](http://sqlninja.sourceforge.net/): Sqlninja is a tool targeted to exploit SQL Injection vulnerabilities on a web application that uses Microsoft SQL Server as its back-end.
- [SSRFTest](https://github.com/daeken/SSRFTest): SSRF testing tool.
- [XSS Hunter](https://xsshunter.com/): XSS Hunter allows you to find all kinds of cross-site scripting vulnerabilities, including the often-missed blind XSS. The service works by hosting specialized XSS probes which, upon firing, scan the page and send information about the vulnerable page to the XSS Hunter service.
- [Ysoserial](https://github.com/frohoff/ysoserial): A proof-of-concept tool for generating payloads that exploit unsafe Java object deserialization.
- [PHP Gadget chain](https://github.com/ambionics/phpggc) 

## Scanners / frameworks

This is a curated list of scanners and frameworks and is not intended to be comprehensive; rather, we want to highlight the tools we find especially useful.

- [Canvas](https://www.immunityinc.com/products/canvas/): CANVAS offers hundreds of exploits, an automated exploitation system, and a comprehensive, reliable exploit development framework to penetration testers and security professionals worldwide.
- [IronWASP](https://resources.infosecinstitute.com/topic/ironwasp-part-1-2): IronWASP (Iron Web Application Advanced Security testing Platform) is an open source tool used for web application vulnerability testing. It is designed in such a way that users having the right knowledge can create their own scanners using this as a framework. IronWASP is built using Python and Ruby and users having knowledge of them would be able to make full use of the platform. However, IronWASP provides a lot of features that are simple to understand.
- [Lazyrecon](https://github.com/nahamsec/lazyrecon): LazyRecon is a script written in Bash, intended to automate the tedious tasks of reconnaissance and information gathering. The information is organized in an html report at the end, which helps you identify next steps.
- [Maltego](https://www.maltego.com/): Maltego is an open source intelligence (OSINT) and graphical link analysis tool for gathering and connecting information for investigative tasks.
- [Metasploit](https://www.metasploit.com/): Metasploit is an open source penetration testing framework.
- [Nikto](https://cirt.net/Nikto2): Nikto is an Open Source (GPL) web server scanner which performs comprehensive tests against web servers for multiple items, including over 6700 potentially dangerous files/programs, checks for outdated versions of over 1250 servers, and version specific problems on over 270 servers.
- [Nmap](https://nmap.org/): Nmap (“Network Mapper”) is a free and open source (license) utility for network discovery and security auditing.
- [OpenVAS](https://www.openvas.org/): OpenVAS is a full-featured vulnerability scanner. Its capabilities include unauthenticated testing, authenticated testing, various high level and low level Internet and industrial protocols, performance tuning for large-scale scans and a powerful internal programming language to implement any type of vulnerability test.
- [Osmedeus](https://github.com/j3ssie/Osmedeus): Osmedeus allows you to automatically run the collection of awesome tools for reconnaissance and vulnerability scanning against the target.
- [Reconness](https://github.com/reconness/reconness): ReconNess helps you to run and keep all your #recon in the same place allowing you to focus only on the potentially vulnerable targets without distraction and without requiring a lot of bash skill, or programming skill in general.
- [Sn1per](https://github.com/1N3/Sn1per): Sn1per Community Edition is an automated scanner that can be used during a penetration test to enumerate and scan for vulnerabilities. Sn1per Professional is Xero Security’s premium reporting addon for Professional Penetration Testers, Bug Bounty Researchers and Corporate Security teams to manage large environments and pentest scopes.
- [Wapiti](https://wapiti.sourceforge.io/): Wapiti allows you to audit the security of your websites or web applications. It performs “black-box” scans (it does not study the source code) of the web application by crawling the web pages of the deployed webapp, looking for scripts and forms where it can inject data.

## Datasets / freemium services

This is a curated list of datasets and freemium services and is not intended to be comprehensive; rather, we want to highlight the tools we find especially useful.

- [C99.nl](https://api.c99.nl/): Subdomain Finder is a scanner that scans an entire domain to find as many subdomains as possible.
- [Censys](https://censys.io/): Censys scans the most ports and houses the biggest certificate database in the world, and provides the most up-to-date,  thorough view of your known and unknown assets.
- [Payloads All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings): A list of useful payloads and bypasses for Web Application Security. Feel free to improve with your payloads and techniques.
- [Rapid7 Forward DNS (FDNS)](https://opendata.rapid7.com/sonar.fdns_v2/): This dataset contains the responses to DNS requests for all forward DNS names known by Rapid7’s Project Sonar.
- [Seclists](https://github.com/danielmiessler/SecLists): SecLists is the security tester’s companion. It’s a collection of multiple types of lists used during security assessments, collected in one place. List types include usernames, passwords, URLs, sensitive data patterns, fuzzing payloads, web shells, and many more. The goal is to enable a security tester to pull this repository onto a new testing box and have access to every type of list that may be needed.
- [Shodan](https://www.shodan.io/): Shodan provides a public API that allows other tools to access all of Shodan’s data. Integrations are available for Nmap, Metasploit, Maltego, FOCA, Chrome, Firefox and many more.

## Miscellaneous hacking tools

This is a curated list of miscellaneous hacking tools and is not intended to be comprehensive; rather, we want to highlight the tools we find especially useful.

- [Altair](https://altair.sirmuel.design/): Altair GraphQL Client helps you debug GraphQL queries and implementations - taking care of the hard part so you can focus on actually getting things done.
- [BuiltWith](https://addons.mozilla.org/en-US/firefox/addon/builtwith/): BuiltWith’s goal is to help developers, researchers and designers find out what technologies web pages are using, which may help them decide what technologies to implement themselves.
- [Ettercap](https://www.ettercap-project.org/): Ettercap is a comprehensive suite which features sniffing of live connections, content filtering, and support for active and passive dissection of many protocols, including multiple features for network and host analysis.
- [Foxyproxy](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/): FoxyProxy is an advanced proxy management tool that completely replaces Firefox’s limited proxying capabilities. For a simpler tool and less advanced configuration options, please use FoxyProxy Basic.
- [John the Ripper](https://www.openwall.com/john/): John the Ripper is free and Open Source software, distributed primarily in a source code form.
- [Swiftness X](https://github.com/ehrishirajsharma/SwiftnessX): A note taking tool for BB and pentesting.
- [THC Hydra](https://github.com/vanhauser-thc/thc-hydra): This tool is a proof-of-concept code, designed to give researchers and security consultants the possibility to show how easy it would be to gain unauthorized access from remote to a system.
- [Transformations](https://transformations.jobertabma.nl/): Transformations makes it easier to detect common data obscurities, which may uncover security vulnerabilities or give insight into bypassing defenses.
- [Wappalyzer](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/): Wappalyzer is a browser extension that uncovers the technologies used on websites. It detects content management systems, eCommerce platforms, web servers, JavaScript frameworks, analytics tools and many more.
- [Wireshark](https://www.wireshark.org/): Wireshark® is a network protocol analyzer that lets you capture and interactively browse the traffic running on a computer network.