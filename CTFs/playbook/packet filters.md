## TCPDUMP
- [TCPDUMP documentation](https://linux.die.net/man/8/tcpdump)
- [Documentation for packet filters](https://linux.die.net/man/7/pcap-filter) 
- TCP Flags are 13th octet in the header with this order: `C|E|U|A|P|R|S|F`
	- Filter to print packets with push flag:  `tcp[13] & 8 != 0`, because `P` is at 4th bit
- Print only data
```sh
# -q to print less header information
# -t to hide the timestamp
# -A to print data in ASCII
# -n, -nn : don't lookup names for host, port 
tcpdump -c 5 -A -i eth0 -n -nn -q -t '(((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```
- Print only SYN/ACK packets
```sh
tcpdump 'tcp[tcpflags] & (tcp-syn|tcp-fin) != 0 and not src and dst net localnet'
```

## TSHARK
- Display only TCP data: `tshark -Y "tcp" -T fields -e tcp.payload | xxd -r -p`

## Firewall
- `iptables --append INPUT --protocol tcp --dport 31337 -i eth0 -j DROP`