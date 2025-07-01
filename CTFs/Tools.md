- wireshark - pcap
- impacket-secretsdump - Identity management: SAM files
- gpp-decrypt - aes decrypt

## Forensics
- `.NET` decompiler: [DotPeek](https://www.jetbrains.com/decompiler/)
- Basic info about windows binary: [PEStudio](https://www.winitor.com/download)
- oledump.py - xlsm
- vol.py - memory dump 
	- get process tree: `python3 vol.py -f ~/dumpfile.raw windows.pstree
- [Nirsoft tool](https://www.nirsoft.net/utils/security_questions_view.html ) - extract security questions in windows registry
- [Fuzzy Hash check - SSDeep](https://ssdeep-project.github.io/ssdeep/index.html)
- OS Fingerprinting:
	- https://lcamtuf.coredump.cx/p0f3/
	- https://github.com/xnih/satori/
- Keyboard Parser
	- https://github.com/TeamRocketIst/ctf-usb-keyboard-parser

## SOC
- [Intrusion Detection MITRE ATT&CK](https://github.com/orgs/mitre-attack/repositories)
- [MITRE - Analytics Repo](https://car.mitre.org/)
- [Event Query Language](https://eql.readthedocs.io/en/latest/)
- [MITRE - Engage](https://engage.mitre.org/) - Adversary engagement approach, [starter kit](https://engage.mitre.org/starter-kit/), [Helping handbook](https://engage.mitre.org/wp-content/uploads/2022/04/EngageHandbook-v1.0.pdf)
- [MITRE - security countermeasures](https://d3fend.mitre.org/)
- [MITRE - threat defense](https://mitre-engenuity.org/cybersecurity/center-for-threat-informed-defense/)
- [Sample ATT&CK emulation plans](https://github.com/center-for-threat-informed-defense/adversary_emulation_library)
- [APT3 emulation plan](https://attack.mitre.org/resources/adversary-emulation-plans/),  [APT29](https://github.com/center-for-threat-informed-defense/adversary_emulation_library/tree/master/apt29), and [FIN6](https://github.com/center-for-threat-informed-defense/adversary_emulation_library/tree/master/fin6).
- 
## Red Team
- C2 framework: 
	- https://github.com/BishopFox/sliver 
- [mac address to vendor API](https://mac2vendor.com/articles/apiv4): `curl https://mac2vendor.com/api/v4/mac/0002DC`
- Out of band tool: https://github.com/projectdiscovery/interactsh
- EDR Check:
	- https://github.com/PwnDexter/Invoke-EDRChecker
	- https://github.com/PwnDexter/SharpEDRChecker 
	- https://github.com/owasp-amass/amass 
- Payload generator
	- https://github.com/trustedsec/nps_payload
	- https://github.com/GreatSCT/GreatSCT
	- https://github.com/offsecginger/koadic
	- https://github.com/trustedsec/unicorn
- Tunneling
	- [Sshuttle](https://github.com/sshuttle/sshuttle)
	- [Rpivot](https://github.com/klsecservices/rpivot)
	- [Chisel](https://github.com/jpillora/chisel)
	- [Hijacking Sockets with Shadowmove](https://adepts.of0x.cc/shadowmove-hijack-socket/)
- [SMB enumeration](https://github.com/CiscoCXSecurity/enum4linux)
## Recon
- OSINT
	- https://github.com/laramies/theHarvester - gather emails, names, subdomains, IPs, and URLs using multiple public data sources
	- https://hunter.io/ - email hunting tool 
	- https://osintframework.com/ - collection of OSINT tools
	- https://osintframework.com/ 
	- https://hunter.io/
	- https://github.com/laramies/theHarvester 
	- https://attack.mitre.org/techniques/T1070/006/
	- https://cloud.projectdiscovery.io/ - discover domain details

- Enumeration
	- [Configuration Enumeration - Seabelt](https://github.com/GhostPack/Seatbelt)
	- [AD enumeration - BloodHound](https://github.com/BloodHoundAD/BloodHound)
	- [List wayback URLs](https://github.com/tomnomnom/waybackurls)
	- `apt install zaproxy` : Owasp ZAP
	- wfuzz - similar to Burp intruder
- API recon
	- `mitmproxy2swagger` python package
	- jwt: https://github.com/ticarpi/jwt_tool.git 
	- Contectual url enum: https://github.com/assetnote/kiterunner
	- Supporting python library: `pip install termcolor cprint pycryptodomex requests`
	- HTTP param recon: https://github.com/s0md3v/Arjun
	- https://github.com/trufflesecurity/trufflehog 


## Network Monitoring Tools

| <br>                                                                                                                                    |                                                                                                                                                                                                              |
| --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Tool                                                                                                                                    | Usage Description                                                                                                                                                                                            |
| **[Nagios](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/quickstart.html)**                                       | A popular open-source software for monitoring systems, networks, and infrastructure. It provides real-time monitoring and alerting for various services and applications.                                    |
| [SolarWinds Network Performance Monitor](https://documentation.solarwinds.com/en/success_center/npm/content/npm_installation_guide.htm) | A comprehensive network monitoring tool that provides real-time visibility into network performance and availability. It includes network mapping, automated network discovery, and customisable dashboards. |
| **[PRTG](https://www.paessler.com/manuals/prtg/installation)**                                                                          | An all-in-one network monitoring tool that provides comprehensive performance and availability monitoring. It includes real-time traffic analysis, custom dashboards, and customisable alerts.               |
| **[Zabbix](https://www.zabbix.com/download)**                                                                                           | A powerful open-source network monitoring tool that provides real-time network performance and availability monitoring. It includes features such as customisable dashboards, network mapping, and alerting. |



tools inside the FlareVM. It has many specialized forensics, incident response, and malware investigation tools.

Below are the tools grouped by their category. 

  

**Reverse Engineering & Debugging**

Reverse engineering is like solving a puzzle backward: you take a finished product apart to understand how it works. Debugging is identifying errors, understanding why they happen, and correcting the code to prevent them.

- **Ghidra** - NSA-developed open-source reverse engineering suite.
- **x64dbg** - Open-source debugger for binaries in x64 and x32 formats.
- **OllyDbg** - Debugger for reverse engineering at the assembly level.
- **Radare2** - A sophisticated open-source platform for reverse engineering.
- **Binary Ninja** - A tool for disassembling and decompiling binaries.
- **PEiD** - Packer, cryptor, and compiler detection tool.

  

**Disassemblers & Decompilers**

Disassemblers and Decompilers are crucial tools in malware analysis. They help analysts understand malicious software’s behaviour, logic, and control flow by breaking it into a more understandable format. The tools mentioned below are commonly used in this category.

- **CFF Explorer** - A PE editor designed to analyze and edit Portable Executable (PE) files.
- **Hopper Disassembler** - A Debugger, disassembler, and decompiler.
- **RetDec** - Open-source decompiler for machine code.

  

**Static &** **Dynamic Analysis**

Static and dynamic analysis are two crucial methods in cyber security for examining malware. Static analysis involves inspecting the code without executing it, while dynamic analysis involves observing its behaviour as it runs. The tools mentioned below are commonly used in this category.

- **Process Hacker** - Sophisticated memory editor and process watcher.
- **PEview** - A portable executable (PE) file viewer for analysis.
- **Dependency Walker** - A tool for displaying an executable’s DLL dependencies.
- **DIE (Detect It Easy)** - A packer, compiler, and cryptor detection tool.

  

**Forensics & Incident Response**

Digital Forensics involves the collection, analysis, and preservation of digital evidence from various sources like computers, networks, and storage devices. At the same time, Incident Response focuses on the detection, containment, eradication, and recovery from cyberattacks. The tools mentioned below are commonly used in this category.

- **Volatility** - RAM dump analysis framework for memory forensics.
- **Rekall** - Framework for memory forensics in incident response.
- **FTK Imager** - Disc image acquisition and analysis tools for forensic use.

  

**Network Analysis**

Network Analysis includes different methods and techniques for studying and analysing networks to uncover patterns, optimize performance, and understand the underlying structure and behaviour of the network.

- **Wireshark** - Network protocol analyzer for traffic recording and examination.
- **Nmap** - A vulnerability detection and network mapping tool.
- **Netcat** - Read and write data across network connections with this helpful tool.

  

**File Analysis**

File Analysis is a technique used to examine files for potential security threats and ensure proper file permissions.

- **FileInsight** - A program for looking through and editing binary files.
- **Hex Fiend** - Hex editor that is light and quick.
- **HxD** - Binary file viewing and editing with a hex editor.

  

**Scripting & Automation**

Scripting and Automation involve using scripts such as PowerShell and Python to automate repetitive tasks and processes, making them more efficient and less prone to human error.

- **Python** - Mainly automation-focused on Python modules and tools.
- **PowerShell** **Empire** - Framework for PowerShell post-exploitation.

  

**Sysinternals Suite**

The Sysinternals Suite is a collection of advanced system utilities designed to help IT professionals and developers manage, troubleshoot, and diagnose Windows systems.

- **Autoruns** - Shows what executables are configured to run during system boot-up.
- **Process Explorer** - Provides information about running processes.
- **Process Monitor** -Monitors and logs real-time process/thread activity.

  

  

|                  |                                                                                                                                   |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Tool**         | **Investigative Value**                                                                                                           |
| Procmon          | A helpful tool for tracking system activity, especially regarding malware research, troubleshooting, and forensic investigations. |
| Process Explorer | Allows you to see the Process of the Parent-child relationship, DLLs loaded, and its path.                                        |
| HxD              | Malicious files can be examined or altered via hex editing.                                                                       |
| Wireshark        | Observing and investigating network traffic to look for unusual activity.                                                         |
| CFF Explorer     | Can generate file hashes for integrity verification, authenticate the source of system files, and validate their validity.        |
| PEStudio         | Static analysis or studying executable file properties without running the files.                                                 |
| FLOSS            | Extracts and de-obfuscates all strings from malware programs using advanced static analysis techniques.                           |