
Automated tools
- **LinPeas**: [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
- **LinEnum:** [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)
- **LES (****Linux** **Exploit Suggester):** [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
- **Linux** **Smart Enumeration:** [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- **Linux** **Priv Checker:** [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)

## Web recon:

1. robots.txt
2. framework discovery using favicon: [https://wiki.owasp.org/index.php/OWASP_favicon_database](https://wiki.owasp.org/index.php/OWASP_favicon_database)
3. sitemap.xml
4. http headers
5. email usernames: [https://github.com/nyxgeek/username-lists/blob/master/usernames-top100/usernames_gmail.com.txt](https://github.com/nyxgeek/username-lists/blob/master/usernames-top100/usernames_gmail.com.txt)

## samba share
- [SMB enumeration - enum4linux](https://github.com/portcullislabs/enum4linux)
	- -U - get userlist  
	- -M - get machine list  
	- -N - get namelist dump (different from -U and-M)  
	- -S - get sharelist  
	- -P - get password policy information  
	- -G - get group and member list
	- -a - all of the above (full basic enumeration)
 - connecting to SMB share
```sh
# -U [name] : to specify the user
# -p [port] : to specify the port
smbclient //[IP]/[SHARE]

```

- Download samba share recursively:
```sh
smbget -R smb://10.10.165.133/anonymous
```

## NFS file system
- Details about NFS:
- [Nlog - what-is-nfs-file-share/](https://www.datto.com/blog/what-is-nfs-file-share/)
- [Archlinux - NFS](https://wiki.archlinux.org/index.php/NFS)
- [Sourceforge - NFS](http://nfs.sourceforge.net/)
- Vulnerability: 
	- root_swashing
- Tools:
	- [https://packages.ubuntu.com/jammy/nfs-common](https://packages.ubuntu.com/jammy/nfs-common)
	- nfs-common: **lockd, statd**, **showmount**, **nfsstat, gssd**, **idmapd** and **mount.nfs**

- mounting the NFS share:

```sh
mkdir /mnt/mountpoint
sudo mount -t nfs IP:share /mnt/mountpoint -nolock

/usr/sbin/showmount -e [IP] # list the NFS shares
```
  

## SMTP
- Explanation: [https://computer.howstuffworks.com/e-mail-messaging/email3.htm](https://computer.howstuffworks.com/e-mail-messaging/email3.htm)
- [SMTP Blog](https://www.afternerd.com/blog/smtp/)
- Tools:
	1. `smtp_version` in metasploit
	2. `smtp_enum` in metasploit - to enumerate users using `VRFY` and `EXPN` commands in smtp
	3. `smtp-user-enum` : [https://www.kali.org/tools/smtp-user-enum/](https://www.kali.org/tools/smtp-user-enum/)

## mysql
- install client: `sudo apt install default-mysql-client`
- tools:
	- [Nmap - mysql-enum.html](https://nmap.org/nsedoc/scripts/mysql-enum.html)
	- [exploit-db - 23081](https://www.exploit-db.com/exploits/23081)
- metasploit modules:
	1. `mysql_schemadump`
	2. `mysql_sql`
	3. `mysql_hashdump`
## subdomains:
- websites:
	- [https://ui.ctsearch.entrust.com/ui/ctsearchui](https://ui.ctsearch.entrust.com/ui/ctsearchui)
	- [https://crt.sh](https://crt.sh)
	- google: `site:*.domain.com -site:[www.domain.com](http://www.domain.com)`

- tools
	- dnsrecon: `dnsrecon -t brt -d acmeitsupport.thm`
	- [Sublist3r](https://github.com/aboul3la/Sublist3r) : `./sublist3r.py -d acmeitsupport.thm`

## ffuf

- ffuf: [https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf)
- Default word to replace: FUZZ
```sh
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.126.217/customers/signup -mr "username already exists"

```

- Custom word to replace: W1, W2
```

ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.126.217/customers/login -fc 200

# Generate 3 digit OTP between 100 to 200
crunch 3 3 -o otp.txt -t %%% -s 100 -e 200 

```

- Filtering
```sh
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.196.243


# Filter by response size
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.196.243 -fs {size}

```

## hydra brute force password:
```sh
hydra -l <username> -P <full path to pass> 10.10.73.242 -t 4 ssh

hydra -l user -P passlist.txt ftp://10.10.73.242

hydra -l <username> -P <wordlist> 10.10.73.242 http-post-form "/:username=^USER^&password=^PASS^:incorrect" -V

hydra -l R1ckRul3s -P /usr/share/wordlists/rockyou.txt 10.10.237.241 http-post-form “/login.php:username=^USER^&password=^PASS^:incorrect" -V

  
hydra -l admin -P ./SecLists/Passwords/500-worst-passwords.txt 'http-get://enum.thm/labs/basic_auth/:A=BASIC:S=200'
```

## GoBuster - path enumeration
```sh
# directory enumeration:** 
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r

# dns enumeration:** 
gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/****DNS****/subdomains-top1million-5000.txt

# vhost enumeration:** 
gobuster vhost -u "http://MACHINE_IP" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320 

```

## Public gitlab repos
```python
import gitlab
import uuid
  
# Create a Gitlab connection
gl = gitlab.Gitlab("http://gitlab.tryhackme.loc/", private_token='REPLACE_ME')
gl.auth()

# Get all Gitlab projects
projects = gl.projects.list(all=True)

# Enumerate through all projects and try to download a copy
for project in projects:
    print ("Downloading project: " + str(project.name))
    #Generate a UID to attach to the project, to allow us to download all versions of projects with the same name
    UID = str(uuid.uuid4())
    print (UID)
    try:
        repo_download = project.repository_archive(format='zip')
        with open (str(project.name) + "_" + str(UID) +  ".zip", 'wb') as output_file:
            output_file.write(repo_download)
    except Exception as e:
        # Based on permissions, we may not be able to download the project
        print ("Error with this download")
        print (e)
        pass
```