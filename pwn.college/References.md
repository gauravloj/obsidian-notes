- [Syscalls](https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/)
- [Rapple](https://github.com/yrp604/rappel) -Check register state for assembly instructions
- [X86 opcode list](http://ref.x86asm.net/coder64.html)
- [X86 assmbly directives](https://www.felixcloutier.com/x86/) - felix coutier
- https://soc.me/interfaces/x86-prefixes-and-escape-opcodes-flowchart
- Debugging
	- - [GDB's documentation](https://sourceware.org/gdb/onlinedocs/gdb/index.html)
	- [Tudor's gdb crash course](https://web.archive.org/web/20250101052732/https://users.umiacs.umd.edu/~tdumitra/courses/ENEE757/Fall15/misc/gdb_tutorial.html)
	- [gdb debugging full example](https://www.brendangregg.com/blog/2016-08-09/gdb-example-ncurses.html)
	- [pwndbg: a gdb extension (feature list)](https://github.com/pwndbg/pwndbg/blob/dev/FEATURES.md)
	- [gef: another gdb extension (feature list)](https://hugsy.github.io/gef/commands/aliases/)
	- The course [Debuggers 1012: Introductory GDB](https://ost2.fyi/Dbg1012) from OpenSecurityTraining2.
- [Kaitai Struct](https://kaitai.io/) - web IDE for binary parsers
- [pwntools cheatsheet](https://gist.github.com/anvbis/64907e4f90974c4bdd930baeb705dedf)
- [Yet another pwntools cheatsheet](https://corgi.rip/posts/pwntools-cheatsheet/)
- [Networkin in C](https://www.binarytides.com/socket-programming-c-linux-tutorial/)
- [Pipes in C](https://jameshfisher.com/2017/02/17/how-do-i-call-a-program-in-c-with-pipes/
- [Advance Bash Scripting](https://www.linuxtopia.org/online_books/advanced_bash_scripting_guide/)
- [Cryptography resources](https://blog.cryptographyengineering.com/useful-cryptography-resources/)
- [GLIBC - Bootlin](https://elixir.bootlin.com)
- Kernel - https://cirosantilli.com/linux-kernel-module-cheat/ 
- 

## Bash
- https://mywiki.wooledge.org/
- https://mywiki.wooledge.org/BashPitfalls
- [Bash manual](https://www.gnu.org/software/bash/manual/html_node/index.html)
- [Shell script security](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ShellScriptSecurity/ShellScriptSecurity.html#//apple_ref/doc/uid/TP40004268-CH8-SW2)
- [Bypass restrictions](https://book.hacktricks.wiki/en/linux-hardening/bypass-bash-restrictions/index.html)
- http://www.etalabs.net/sh_tricks.html
- 

## Learning
- [Vulnerabilit 1001 - OST2](https://p.ost2.fyi/courses/course-v1:OpenSecurityTraining2+Vulns1001_C-family+2023_v1/about)
- [Vulnerabilit 1002 - OST2](https://p.ost2.fyi/courses/course-v1:OpenSecurityTraining2+Vulns1002_C-family+2023_v1/about)


## Networking
- [`ip` cheatsheet](https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf)
- [Python Struct doc](https://docs.python.org/3/library/struct.html#format-characters)
- 

## Static Analysis


- kaitai struct ([https://ide.kaitai.io/](https://ide.kaitai.io/)): file format parser and explorer
- nm: lists symbols used/provided by ELF files
- strings: dumps ASCII (and other format) strings found in a file
- objdump: simple disassembler
- checksec ([https://github.com/slimm609/checksec.sh](https://github.com/slimm609/checksec.sh)): analyzes security features used by an executable

- Disassemblers
	- [IDA Pro: the "gold standard" of disassemblers ](https://www.hex-rays.com/products/ida/)
	- [Binary Ninja:](https://binary.ninja/) IDA's main commercial competitor 
	- [Binary Ninja Cloud](https://cloud.binary.ninja/): a version of Binary Ninja that runs in your browser! 
	- [angr management](https://github.com/angr/angr-management/releases): an academic binary analysis framework! 
	- [ghidra](https://ghidra-sre.org/) : a reversing tool created by the National Security Agency 
	- [cutter](https://cutter.re/): a reversing tool created by the radare2 open source project
