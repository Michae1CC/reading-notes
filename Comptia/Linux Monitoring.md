
- `netstat -i` lists interfaces
- `iptables -nvL` Gives information and statistics about what's being received on particular input chains or input rules
- linux `watch -d` TODO find out

### ifconfig

- `sudo ifconfig eth0 192.168.0.4` Changes the ip address for the specified interface
- `sudo ifconfig add eth0 192.168.0.4` Adds the ip address for the specified interface (does no persist when the interface is shutdown)
- `sudo ifconfig eth0 down; sudo ifconfig eth0 up` enable/disable an interface

### ip

- `ip address show`, `ip -4 address show` (ipv4 only)
- `ip route show`
- `ip neighbour show`
- `sudo ip link set eth0 up`

### route

Route manipulates the kernel's IP routing tables. Its primary use is to set up static routes to specific hosts or networks.

- `route` or `ip route show`
- `route -n` Displays the network numbers rather than the network names
- The file `/etc/networks` on Linux systems is a plain-text file that lists symbolic names for IP networks. It primarily serves to translate between network names and their corresponding IP address ranges for tools that need to display this information, like the (now deprecated) `route` command.

### arp

- Reads from the `/proc/net/arp` file which contains the arp cache
- Adding entires `arp -s <ip> <mac>`
- Deleting entries `arp -d <ip> | <mac>`


### Configuring WiFi from Command Line

- `/etc/wpa_supplicant/wpa_supplicant.conf` store wifi configuration
- `sudo iwlist wlan0 scan` Scans the area for wifi
- `iwconfig wlan0` Shows us the configuration of our wlan0 adapter
- `sudo wpa_cli status`


### tcpdump

- Command line packet analyser. Can also interrogate the packets.
- `sudo tcpdump -i eth0`, Display all packets on eth0
- `sudo tcpdump -c 5 -i eth0` Only capture 5 packets
- `sudo tcpdump -i eth0 not port 22` Don't capture SSH traffic, useful when remoting into the host

### netstat

- Commonly used to display open ports on a host and is found on Unix, Linux and Windows hosts
- `-a` Show all
- `-n` Numeric
- `-r` routes
- `-l` Listening
- `-t` TCP
- `-x` Sockets


### nmap

- A port scanner, used for network explorations and security auditing; being able to scan large networks or single hosts identifying open ports and operating system versions
- tcp scan can be initiated by standard user and use the UNIX connect libraries. As such a full TCP 3 way handshake is initiated. These scans are easy to detect and mitigate against.
- `--iflist` list interface info
- The SYN scan requires root privileges on the system using nmap. The connection is broken down on the receipt of a SYN packet from the target port. Modern firewalls can detect SYN scans but it is made more difficult in the way nmap alters timings
- The version of the service hosting port can be detected with `-sV`
- OS detection `-A` (agressive scan)
- `/etc/resolv.conf` to see which dns servers are set

### Managing Hostnames

- The persistent hostname of a system is stored in:
	- `/etc/HOSTNAME` - SUSE
	- `/etc/hostname` - Debian
- The `hostname` - sets the transient hostname set from the hostname file and can be displayed using the hostname command
- The root user can change the transient name whilst the system is running but will not persist reboots
- The hostname can be controlled via systemd using hostnamectl
- `id` command


### Consistent Naming Scheme
- Previously inconsistent naming scheme: eth0, eth1, has been replaces with the consistent naming scheme. This is where the address of the card 