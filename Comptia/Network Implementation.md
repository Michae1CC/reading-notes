
Layer 2 Loop
- Adding two link between two switches can be dangerous since it can introduce a layer 2 loop where broadcast messages are perpetually sent and received on alternating
- Spanning tree protocol can prevent this problem by blocking one of the ports. It finds loops in a L2 network and blocks one of the ports.

Link Aggregation
- Aims to solve the problem of having two links connecting two switches together to create a faster link so we can send more traffic over it.
- Link Aggregation Control Protocol (LACP), also goes by:
	- EtherChannel
	- Port Channel
	- Aggregate Ethernet

Wireless Channels
- Channel - A range of frequencies from the electromagnetic spectrum used to transmit information
- When we have multiple access points in our network we keep each access point on its own unique channel to allow client to unambiguously communicate to an access point

Wireless Network Types and Antennas
- Yagi antenna - used for p2p communication. Prevents the use of copper wire or other physical connection
- Wireless mesh network - Wirelessly extends network in homes

SSID
- Service Set Identifier (SSID) - Wi-Fi Network Name
- Basic Service Set Identifier (BSSID) - MAC Address of the Access Point we are associated with
- Extended Service Set Identifier (ESSID) - Used in a Multiple Access Point Network. Wi-Fi network name

Wireless Encryption
- WPA3 - 192 bit encryption, additional integrity for handshake

Network Racks
- Main Distribution Frame - A rack used to bring in all the external connections from outside an office building into the building. Uses a fiber optic patch panel.
- The main distribution frame is going to connect to other frames or racks in our building called intermediate distribution frames, or IDFs.

