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

