
Methodology
- Identify the Problem
- Establish a theory of probable cause
- Test the theory to determine the cause
- Establish a plan of action to resolve and identify potential effects
- Implement a solution or escalate
- Verify full system functionality and implement preventative measures if needed
- Document findings, actions, outcomes and lessons learned


#### Subnet Mask and IP Address Troubleshooting

- Whenever you see APIPA address assigned to network interfaces, generally something is wrong

Tools
- `tcpdump -i 1` can create `pcapng` files which can be analysed in Wireshark
- `netstat` - allows us to examine network things on our local device.
	- `-r` Provides a route table configuration for device
	- `-an` Lists ports that are currently in use (listening or connected). Use `-p` to print port.
	- `-i` Lists interfaces (linux only).
- `nmap` - Allows us to scan a remote system for networking.

Tools for Network Hardware
- Link Layer Discovery Protocol - allows us to map out devices on network. `show lldp neighbors`

Switch port statuses
- Up - The switch port is passing traffic
- Down - switch port is not passing traffic, but if we plug something into it, it has the potential to go to the up state
- Administratively down - Issue a command to prevent the port from passing traffic