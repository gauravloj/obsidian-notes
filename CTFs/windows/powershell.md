  

- Command format: **Verb-Noun**
- List of verbs: [https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands?view=powershell-7](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands?view=powershell-7)

  
```

# Help on a command

Get-Help command-name

  

# Get example of command

Get-Help command-name -Examples

  

# List of all commands

Get-Command

  

# List commands using pattern

Get-Command Verb-*

Get-Command *-Noun

  

# Powershell passes output objects, instead of raw text, between two commands

# List the methods and properties of output object

Command | Get-Member

  

# Select specific property from the object

# Here 'name' and 'mode' are properties of previous command

Command | Select-Object -Property mode,Name

  

  

# Filtering object:

Verb-Noun | Where-Object -Property PropertyName -operator Value`

  

# Another version 

# the $_ operator to iterate through every object passed to the Where-Object _cmdlet_.

# Operator list: [https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-6](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-6)

Verb-Noun | Where-Object {$_.PropertyName -operator Value}

  

# Sort object

Command | Sort-Object


# Base64 conversion

$text = Get-Content .\b64.txt

$outbytes = [System.Convert]::FromBase64String($text)

[System.Text.Encoding]::ASCII.GetString($outbytes)

  

# Find hash

Get-Filehash -Algorithm md5 ./filename.txt

  

# Find file

Get-ChildItem -Path C:\ -Include *.txt -File -Recurse -ErrorAction SilentlyContinue

  

# Find a string in any file

Get-ChildItem C:\* -recurse | Select-String -pattern "StringToFind"

  

# Get detailed info of files

Get-ChildItem | Format-List *

  

  

# Enumeration

  

# List user accounts

Get-LocalUser

  

# Get local groups

get-localgroup

  

# Get ip info

get-netipaddress

  

# Get active network listening connections

Get-NetTCPconnection -State listen

  

# List applied patches

get-hotfix

  

# List running process

get-process

  

# List scheduled tqask

Get-ScheduledTask

  

# List file/dir access permissions

Get-Acl

  

# Ping multiple hosts

1..15 | %{ echo "10.0.2.$_"; ping -n 1 10.0.2.$_ | select-string ttl}

  

# Check open ports

1..15 | %{ echo ((New-Object Net.Sockets.TcpClient).connect("10.0.2.8",$_ )) "Open port on - $_" } 2> $null

  

# Get history of commands

cat (Get-PSReadlineOption).HistorySavePath



  ```

### Enumerate using powerview

 - [PowerView.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/dev/Recon/PowerView.ps1)
-  [basic-powershell-for-pentesters/powerview.html](https://book.hacktricks.wiki/en/windows-hardening/basic-powershell-for-pentesters/powerview.html)

  
```
Import-Module PowewrView.ps1

# Get domain controller info

**Get-NetDomainController**

  

**#** Get a list of domain users

**Get-NetUser**

  

  
# enumerate systems connected to the domain.

**Get-NetComputer**

  

# use “-ping” parameter to enumerate the systems that are currently online.

**Get-NetComputer -ping**

  

# Enumerate existing groups

Get-NetGroup

  

# Enumerate members of the group

Get-NetGroupMember "Domain Admins"

  

# list available shares.

Find-DomainShare

  

# add "-CheckShareAccess" to list only readable shares. 

Find-DomainShare **-**CheckShareAccess

  

# gather information on enforced policies

Get-NetGPO

  

# list any domain you may access

Get-NetDomainTrust

  

# Run powerview command on other domain

Get-NetUsers -Domain infra.munn.local

  

# Find systems that can be accessed as local admin

Find-LocalAdminAccess

  

  
```


### Other useful commands
```powershell
  
# Start new process

Start-Process notepad.exe

  

# List running processes

Get-Process -name notepad

  

# Save output in csv file

Get-Process | Export-Csv running-process.csv

  

# Save output in a file

Get-Process | Out-file running-process.txt

  

# Check other output options

Get-command Out-*

  

# Display file content

Get-Content filename

  

# Copy/move files

Copy-Item src-path dst-path

Move-Item src-path dst-path

  

# Get powershell execution policy

Get-ExecutionPolicy -list

  

  

# Run file with bypass execution

powershell -ExecutionPolicy Bypass -File powershellscript.ps1

  

# Bypass for whole session

Set-ExecutionPolicy Bypass -Scope process

  

# Download files

(New-Object System.Net.WebClient).Downloadfile('fileurl', 'output-filename')

  

Invoke-WebRequest 'file-url' -OutFile 'output-filename'
```