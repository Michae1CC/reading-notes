
- Looking at the Capture file properties under statistics can be useful for view information about a capture file
- Conversations are split by the OSI layer we are operating at
- Conversations can be applied as a filter
- Profiles let you customise different layouts
	- Can drag data into the columns to create new columns

### Display filters

- or logical operator: `or` or `||`, eg. `arp or ip`
- and logical operator: `and` or `&&`, eg. `(icmp and frame contains "abc") or telnet`
- right click on column and apply as filter
- can also right click in packet details to apply as filter
- not operator `!`, eg. `!tcp.port == 80`, `tcp.port != 80`
- Capture filter example: `host 8.8.8.8`. Will cause you to loose packets

### TCP Analysis

- MTU - Ethernet frames has a default max size of 1500 bytes of data
- An IP packet is the L3 payload, the IP header is 20 bytes => 1500 -20 bytes = 1480 bytes for maximum payload size
- Segment is the L4 data that may have been segmented by the L4 protocol
- 1460 - 20 = 1460 bytes. This is the max MSS
- Devices send TCP RST packets to indicate the end of the session. Devices like firewalls and load balancers are lookin for those RST flags in the TCP header to shutdown connection.

### TCP Troubleshooting

- ECN-Echo: Set- indicates congestion was reported
- CWR: Set - indicates congestion window was reduced - sender ack congestion
- Wireshark expert information can be used to gather warning and errors in packets