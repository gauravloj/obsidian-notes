Username: andrea.mitchell Password: Password1

- `ssh za.tryhackme.com\\andrea.mitchell@thmjmp1.za.tryhackme.com`

## Credential Injection
- `runas.exe` : Given AD credentials, run any command as the given username
- Eg. `runas.exe /netonly /user:<domain>\<username> cmd.exe`
- SYSVOL is a folder that exists on all domain controllers. It is a shared folder storing the Group Policy Objects (GPOs) and information along with any other domain related scripts. It is accessible by everyone.
- To access SYSVOL, DNS needs to be configured. In the new command prompt:
```powershell
$dnsip = "<DC IP>"
$index = Get-NetAdapter -Name '<Ethernet>' | Select-Object -ExpandProperty 'ifIndex'
Set-DnsClientServerAddress -InterfaceIndex $index -ServerAddresses $dnsip
```
- Check SYSVOL, if it succeeds then the credentials are working
```powershell
# dir \\<domain>\SYSVOL\
dir \\za.tryhackme.com\SYSVOL\
```
-  _difference between_ _`dir \\za.tryhackme.com\SYSVOL` and `dir \\<DC IP>\SYSVOL`_ :  For hostname, network authentication will attempt first to perform Kerberos authentication. Since Kerberos authentication uses hostnames embedded in the tickets, if we provide the IP instead, we can force the authentication type to be NTLM.
- Once this is done,  With the `/netonly` option, all network communication will use these injected credentials for authentication. This includes all network communications of applications executed from that command prompt window.

## Microsoft Management Console (MMC)

- MMC with  [Remote Server Administration Tools'](https://docs.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps) (RSAT) AD Snap-Ins
- Enable RSAT:
	1. Press **Start**
	2. Search **"Apps & Features"** and press enter
	3. Click **Manage Optional Features**
	4. Click **Add a feature**
	5. Search for **"RSAT"**
	6. Select "**RSAT: Active Directory Domain Services and Lightweight Directory Tools"** and click **Install**
- Start the mmc using from `run` console instead of direct search to use the injected credentials
- In MMC, attach the AD RSAT Snap-In:
	1. Click **File** -> **Add/Remove Snap-in**
	2. Select and **Add** all three Active Directory Snap-ins
	3. Click through any errors and warnings 
	4. Right-click on **Active Directory Domains and Trusts** and select **Change Forest**
	5. Enter _za.tryhackme.com_ as the **Root domain** and Click **OK**
	6. Right-click on **Active Directory Sites and Services** and select **Change Forest**
	7. Enter _za.tryhackme.com_ as the **Root domain** and Click OK
	8. Right-click on **Active Directory Users and Computers** and select **Change Domain**
	9. Enter _za.tryhackme.com_ as the **Domain** and Click **OK**
	10. Right-click on **Active Directory Users and Computers** in the left-hand pane  
	11. Click on **View** -> **Advanced Features**
- It will show a lot of details about AD domain for enumeration

## Enumerate using command prompt
- List all domain users: `net user /domain`
- Detail about single user: `net user zoe.marshall /domain`
- List groups in domain: `net group /domain`
- Details about single domain: `net group "Admins" /domain`
- password policy of the domain: `net accounts /domain`
## Enumeration using Powershell
- [List of Active Directory cmdlets](https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps)
- Get user details: `Get-ADUser -Identity gordon.stevens -Server za.tryhackme.com -Properties *`
- Filter and format the results: `Get-ADUser -Filter 'Name -like "*stevens"' -Server za.tryhackme.com | Format-Table Name,SamAccountName -A`
- Groups enumeration: `Get-ADGroup -Identity Administrators -Server za.tryhackme.com`
- Enumerate group membership: `Get-ADGroupMember -Identity Administrators -Server za.tryhackme.com`
- Generic search for AD object
```powershell
$ChangeDate = New-Object DateTime(2022, 02, 28, 12, 00, 00)

Get-ADObject -Filter 'whenChanged -gt $ChangeDate' -includeDeletedObjects -Server za.tryhackme.com
```
- Get domain details: `Get-ADDomain -Server za.tryhackme.com`
- Altering AD objects: `Set-ADAccountPassword -Identity gordon.stevens -Server za.tryhackme.com -OldPassword (ConvertTo-SecureString -AsPlaintext "old" -force) -NewPassword (ConvertTo-SecureString -AsPlainText "new" -Force)`

## Using BloodHound
- [BloodHound - github](https://github.com/BloodHoundAD/BloodHound)
- BloodHound is GUI to display AD host connections in Visual graphs
- SharpHound is the script to run the enumeration
	- [Command references](https://bloodhound.readthedocs.io/en/latest/data-collection/sharphound-all-flags.html)
	- **Sharphound.ps1** - PowerShell script for running Sharphound. It is good to use with RATs since the script can be loaded directly into memory, evading on-disk AV scans.  
	- **Sharphound.exe** - A Windows executable version for running Sharphound.
	- **AzureHound.ps1** - PowerShell script for running Sharphound for Azure (Microsoft Cloud Computing Services) instances. Bloodhound can ingest data enumerated from Azure to find attack paths related to the configuration of Azure Identity and Access Management.
```
# cd to the directory where the results should be stored
Sharphound.exe --CollectionMethods <Methods> --Domain za.tryhackme.com --ExcludeDCs
```

- Results from above step can be imported in BloodHound for visualization
- Bloodhound uses Neo4j as its backend database and graphing system
- Start Neo4j before starting bloodhound: `neo4j console start`
- Start bloodhound: `bloodhound --no-sandbox`, default creds: `neo4j:neo4j`
- [Exploit technique for different attack vectors](https://bloodhound.readthedocs.io/en/latest/data-analysis/edges.html)

## Other Enumeration techniques
- **[LDAP enumeration](https://book.hacktricks.xyz/pentesting/pentesting-ldap)** - Any valid AD credential pair should be able to bind to a Domain Controller's LDAP interface. This will allow you to write LDAP search queries to enumerate information regarding the AD objects in the domain.
- **[PowerView](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1)** - PowerView is a recon script part of the [PowerSploit](https://github.com/PowerShellMafia/PowerSploit) project. Although this project is no longer receiving support, scripts such as PowerView can be incredibly useful to perform semi-manual enumeration of AD objects in a pinch.
- **[Windows Management Instrumentation (WMI)](https://0xinfection.github.io/posts/wmi-ad-enum/)** - WMI can be used to enumerate information from Windows hosts. It has a provider called "root\directory\ldap" that can be used to interact with AD. We can use this provider and WMI in PowerShell to perform AD enumeration.