
## Physical Security 
- Set GRUB password to avoid gaining boot access in case someone gains physical access to device
```sh
grub2-mkpasswd-pbkdf2
```

## Encrypting file system

```sh
# Get the device ID using any of these commands
fdisk -l
sudo lsblk
sudo blkid

# Set up the partition for LUKS encryption
sudo cryptsetup -y -v luksFormat /dev/sdb1

# Create a mapping to access the partition
sudo cryptsetup luksOpen /dev/sdb1 EDCdrive

# Confirm mapping details using any of these commands
ls -l /dev/mapper/EDCdrive
cryptsetup -v status EDCdrive

# Optional: overwrite existing data with 0s
dd if=/dev/zero of=/dev/mapper/EDCdrive

# Format the partition
sudo mkfs.ext4 /dev/mapper/EDCdrive -L "My Encrypted USB"

# Mount it and start using it like a usual partitio
sudo mount /dev/mapper/EDCdrive /media/secure-USB

# Optional: check the LUKS setting 
sudo cryptsetup luksDump /dev/sdb1
```

## Setup Firewall

- `netfilter` - core  service in kernel that provides packet filtering
- `iptables`, `nftables` - tools to use netfilter

```sh
# --- iptables ---
# Accept incoming request on port 22
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Accept outgoing request on port 22
iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT

# Block everything else
iptables -A INPUT -j DROP
iptables -A OUTPUT -j DROP


# --- nftables ---

# Add a table to add rules to
nft add table fwfilter

# add an 'input' chain and an 'output' chain 
# for incoming and outgoing packets, respectively.
nft add chain fwfilter fwinput { type filter hook input priority 0 \; }
nft add chain fwfilter fwoutput { type filter hook output priority 0 \; }

# add the necessary rule to allow SSH traffic.
# accepts TCP traffic to the local 
# system’s destination port 22.
ft add fwfilter fwinput tcp dport 22 accept

# accepts TCP traffic from the local
# system’s source port 22.
nft add fwfilter fwoutput tcp sport 22 accept

# Check the filters
nft list table fwfilter
```

- `ufw` , `firewalld`: easier way to manage firewall rules

```sh
# Allow ssh traffic
ufw allow 22/tcp

# Check rule status
sudo ufw status


```

# Process Level access control
- https://github.com/SELinuxProject
- https://www.apparmor.net/

## Remote access
- Enable SSH connection for encrypted remote access
- Disable root login for ssh : `PermitRootLogin no` in `sshd_config` file
- Enable key based access: `PubkeyAuthentication yes`, `PasswordAuthentication no`

## User access
- Use sudo users instead of root
- Add user to sudoer's group: `usermod -aG sudo username`
- Disable root login, modify the `/etc/passwd` and change the `root` shell to `/sbin/nologin`.
- Strong password: `libpwquality` config in `/etc/pam.d/common-password`. Install using `apt-get install libpam-pwquality`
- Disable unused accounts

## Software and service
- Disable unused services
- Block unwanted network ports
- Avoid legacy protocols like telnet, ftp
- Remove Identification Strings on remote connection

## Software upgrades

- `apt update`, `apt upgrade`
- 

## Logs
- `/var/log/messages` - a general log for Linux systems
- `/var/log/auth.log` - a log file that lists all authentication attempts (Debian-based systems)
- `/var/log/secure` - a log file that lists all authentication attempts (Red Hat and Fedora-based systems)
- `/var/log/utmp` - an access log that contains information regarding users that are currently logged into the system
- `/var/log/wtmp` - an access log that contains information for all users that have logged in and out of the system
- `/var/log/kern.log` - a log file containing messages from the kernel
- `/var/log/boot.log` - a log file that contains start-up messages and boot information
