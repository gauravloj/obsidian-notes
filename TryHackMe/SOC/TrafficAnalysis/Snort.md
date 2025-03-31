- Website: https://www.snort.org/
- [Snort Manual](http://manual-snort-org.s3-website-us-east-1.amazonaws.com/)
- Open-Source Intrusion Detection and Prevention System

```sh
# Check if snort is installed
snort -V

# Validate config file
sudo snort -c /etc/snort/snort.conf -T

```

- [Snort Rules](https://docs.snort.org/rules/) 
- Eg. `alert tcp any any <> any any (msg:"SYN packet"; sameip; sid:1000001; rev:1;)`
- Snort sniffer mode
	- `v` - Verbose. Display the TCP/IP output in the console.
	- `d` - Display the packet data (payload).
	- `e` - Display the link-layer (TCP/IP/UDP/ICMP) headers.
	- `X` - Display the full packet details in HEX.
	- `i` - define a specific network interface to listen/sniff.
- Snort packet logger mode:
	- `-l` - Logger mode, target **log and alert** output directory. Default output folder is `/var/log/snort`, The default action is to dump as tcpdump format in `/var/log/snort`
	- `-K ASCII` -Log packets in ASCII format.
	- `-r` - Reading option, read the dumped logs in Snort.
	- `-n` - Specify the number of packets that will process/read. Snort will stop after reading the specified number of packets.
	- Eg. `sudo snort -dev -l .`
- Snort IDS/IPS:
	- Enable IPS: Add these flag to snort command `-Q --daq afpacket`

| **Parameter** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -c            | Defining the configuration file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| -T            | Testing the configuration file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **-N**        | Disable logging.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **-D**        | Background mode.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **-A**        | Alert modes;  <br><br>**full:** Full alert mode, providing all possible information about the alert. This one also is the default mode; once you use -A and don't specify any mode, snort uses this mode.<br><br>**fast:**  Fast mode shows the alert message, timestamp, source and destination IP, along with port numbers.<br><br>console: Provides fast style alerts on the console screen.<br><br>**cmg:** CMG style, basic header details with payload in hex and text format.<br><br>**none:** Disabling alerting. |
- Snort pcap analysis:
- `-r / --pcap-single=` - Read a single pcap
- `--pcap-list=""` - Read pcaps provided in command (space separated).
- `--pcap-show` - Show pcap name on console during processing.
