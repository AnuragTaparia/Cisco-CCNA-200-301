
![[Ethernet Frame.png]]

- **Preamble**
	- Length: 7 bytes (56 bits)
	-  It's a series of alternating 1’s and 0’s
	- 10101010 * 7 (this is 1 byte * 7)
	- The purpose of this is that it allows devices to synchronize their receiver clocks to make sure that they are ready to recieve the rest of the frame and the data inside
- **SFD**
	- ‘Start Frame Delimiter’
	- Length: 1 byte (8 bits)
	- 10101011, similar to preamble but the last bit is '1' not '0'
	- Marks the end of the preamble, and the beginning of the rest of the frame
- **Destination & Source**
	- Indicate the devices sending and receiving the frame
	- Consist of the destination and source ‘MAC address’
	-  MAC = Media Access Control
	- The MAC address is a  6 byte (48-bit) address of the physical device
	- The MAC address is assigned to  the device when it is made
- **Type / Length** 
	- 2 byte (16-bit) field
	-  A value of 1500 or less in this field indicates the LENGTH of the encapsulated packet (in bytes)
	-  A value of 1536 or greater in this field indicates the TYPE of the encapsulated packet (usually IPv4 or IPv6), and the length is determined via other methods like
	- IPv4 = 0x0800 (hexadecimal)    IPv6 = 0x86DD (hexadecimal)
	   (2048 in decimal)                       (34525 in decimal) 

- **FCS**
	- ‘Frame Check Sequence’
	-  4 bytes (32 bits) in length
	-  Detects corrupted data by running a ‘CRC’ algorithm over the received data
	-  CRC = ‘Cyclic Redundancy Check’

##### **MAC Address**
- 6-byte (48-bit) physical address assigned to the device when it is made
-  A.K.A. ‘Burned-In Address’ (BIA)
-  The MAC address is globally unique, there are MAC addresses known as locally unique MAC addresses, which don't have to be globally unique
-  The first 3 bytes are the OUI (Organizationally Unique Identifier), which is assigned to the company making the device
-  The last 3 bytes are unique to the device itself
- Written as 12 hexadecimal characters