
## Network Enumeration
- `netstat -an`:  TCP and UDP open ports
- `arp -a`: List of IP/MAC addresses of frequently accessed machines

## Active Directory

Key terms for AD
- Domain Controllers
- Organizational Units
- AD objects
- AD Domains
- Forest
- AD Service Accounts: Built-in local users, Domain users, Managed service accounts
- Domain Administrators

```powershell
# if we get WORKGROUP in the domain section,
# then it means that this machine is part of a local workgroup.
systeminfo | findstr Domain
```

## Users and Groups

AD domain admin accounts

| BUILTIN\Administrator | Local admin access on a domain controller                  |
| --------------------- | ---------------------------------------------------------- |
| Domain Admins         | Administrative access to all resources in the domain       |
| Enterprise Admins     | Available only in the forest root                          |
| Schema Admins         | Capable of modifying domain/forest; useful for red teamers |
| Server Operators      | Can manage domain servers                                  |
| Account Operators     | Can manage users that are not in privileged groups         |


```powershell
# Get all AD users
Get-ADUser -Filter *


# Searching using info in distinguished name
Get-ADUser -Filter * -SearchBase "CN=Users,DC=THMREDTEAM,DC=COM"
```

# Host Security

```powershell

# Check for antivirus from cmd.exe
wmic /namespace:\\root\securitycenter2 path antivirusproduct

# Check for antivirus from cmd.exe
Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntivirusProduct

# Check service status of windows defender
Get-Service WinDefend

# Check if WinDefend is enabled or not
Get-MpComputerStatus | select RealTimeProtectionEnabled

# threats details that have been detected using MS Defender.
Get-MpThreat

# Check firewall enabled status
Get-NetFirewallProfile | Format-Table Name, Enabled

# Check firewall configs
Get-NetFirewallProfile 

# If admin, disable firewall profile
Set-NetFirewallProfile -Profile Domain, Public, Private -Enabled False

# Check firewall rules
Get-NetFirewallRule | select DisplayName, Enabled, Description

# Test TCP connection Test-NetConnection
Test-NetConnection -ComputerName 127.0.0.1 -Port 80

# Test TCP connection using TcpClient
(New-Object System.Net.Sockets.TcpClient("127.0.0.1", "80")).Connected


```

- Event logs
- [EventLog via powershell](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-eventlog?view=powershell-5.1)

```powershell
# List available event logs
Get-EventLog -List

# Check if sysmon is running
Get-Process | Where-Object { $_.ProcessName -eq "Sysmon" }
# or
Get-CimInstance win32_service -Filter "Description = 'System Monitor service'"
# or 
Get-Service | where-object {$_.DisplayName -like "*sysm*"}

# or
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\Microsoft-Windows-Sysmon/Operational

# Find sysmon config
findstr /si '<ProcessCreate onmatch="exclude">' C:\tools\*

```


## Application and services

```
wmic product get name,version

Get-ChildItem -Hidden -Path C:\Users\kkidd\Desktop\

# Check for listening services
net start

# Search the exact service name
wmic service where "name like 'THM Demo'" get Name,PathName

# Get process details
Get-Process -Name thm-demo

# From the process ID above, 
# find the listening ports
netstat -noa |findstr "LISTENING" |findstr "3212"

```

```nslookup
# Change the DNS server
server MACHINE_IP

# List the domains  din different domain controller
ls -d thmredteam.com
```