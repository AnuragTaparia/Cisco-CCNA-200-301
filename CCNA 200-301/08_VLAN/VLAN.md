
VLAN (part 1)

### **What is a LAN?**
- A LAN is a single **broadcast domain**, including all devices in that broadcast domain
- A **broadcast domain** is the group of devices which will receive a broadcast frame (destination MAC FFFF.FFFF.FFFF) sent by any one of the members.
- Broadcast frame = destination MAC FFFF.FFFF.FFFF

### **What is a VLAN?**
- **VLANs (Virtual LANs)** are logical grouping of devices in the same broadcast domain.
- VLANs can spread across multiple switches, with each VLAN being treated as its own subnet or broadcast domain. 
- This means that frames broadcasted onto the network will be switched only between the ports within the same VLAN.
- A VLAN acts like a physical LAN, but it allows hosts to be grouped together in the same broadcast domain even if they are not connected to the same switch.
- Here are the main reasons why VLANs are used:
	- VLANs reduce security risks by reducing the number of hosts that receive copies of frames that the switches flood.
	- you can keep hosts that hold sensitive data on a separate VLAN to improve security.
	- you can create more flexible network designs that group users by department instead of by physical location.
	- network changes are achieved with ease by just configuring a port into the appropriate VLAN.


![](without%20vlan.png)

- Let's say PC in Engineering dept. wants to send broadcast message to all PCs in it's dept. , but since it's a  broadcast message and switch is only aware up to layer 2 (It looks at layer two information like source and destination MAC addresses only.  It doesn't care about layer three, four, etc. So even though there are three separate subnets here).
- Here this is where VLANs comes in

![](vlan.png)

- Although these pieces are all in the same LAN local area network, we can use VLANs or virtual local area networks to separate them at layer two.
- **How exactly do we assign these hosts to VLANs?**
	- We configure them on the switch.
	- More specifically on the switch interfaces, you configure the switch interface to be in a specific VLAN and then the end host connected to that interface is part of that VLAN.
	- The switch will consider each VLAN as a separate LAN and will not forward traffic between VLANs, including broadcast or unknown unicast traffic.
	- We configure them on the switch. More specifically on the switch interfaces, you configure the switch interface to be in a specific VLAN and then the end host connected to that interface is part of that VLAN. 
	- The switch will consider each VLAN as a separate LAN and will not forward traffic between VLANs, including broadcast or unknown unicast traffic.

- So if we have set up these VLANs, if PC 1 sends this broadcast frame after the frame arrives at the switch, it will be forwarded to all interfaces in the same VLAN. Because the broadcast arrived on an interface configured in VLAN ten the switch.

- The switch does not perform inter-VLAN routing. It must send the traffic through the router. 


### **VLAN Configuration**
- VLANs 1,1002-1005 exist by default and cannot be deleted.
- An **access port** is a switchport which belongs to a single VLAN, and usually connects to end hosts like PCs. This type of interface is configured on switch ports that are connected to end devices such as workstations, printers, or access point
- Switchports which carry multiple VLANs are called **trunk ports**.
- By default all VLAN are allowed on trunk port

---
---

VLAN (part 2)

- There is no link in VLAN20 between SW1 and SW2. This is because there are no PCs in VLAN20 connected to SW1. PCs in VLAN20 can still reach PCs connected to SW1, R1 will perform inter-VLAN routing
- Below image demonstrate how router will perform inter-VLAN routing
- ![](router%20performing%20inter-VLAN.png)  
- In a small network with few VLANs, it is possible to use a separate interface for each VLAN when connecting switches to switches, and switches to routers. 
- However, when the number of VLANs increases, this is not viable. It will result in wasted interfaces, and often routers won’t have enough interfaces for each VLAN.
- You can use trunk ports to carry traffic from multiple VLANs over a single interface
![](trunk%20port.png)
- Let's say this PC in vlan 10 wants to send some data to this other PC in VLAN 10. It sends the traffic to SW2, which then sends it to SW1.  
- Now here's a question: how does SW1 know which VLAN the traffic belongs to?
- Both VLANs 10 and 30 are allowed on the interface that traffic was received on.
- But how does the SW1 know which VLAN it belongs to?
- The answer is VLAN tagging
- Switches will ‘tag’ all frames that they send over a trunk link. This allows the receiving switch to know which VLAN the frame belongs to.
- Trunk ports = ‘tagged’ ports 
  Access ports = ‘untagged’ ports 
- frames sent over access ports aren't tagged. They don't need to be tagged because the interface belongs to a single VLAN.


### VLAN Tagging

- There are two main trunking protocols: ISL (Inter-Switch Link) and IEEE 802.1Q. 
- ISL is an old Cisco proprietary protocol created before the industry standard IEEE 802.1Q. 
- IEEE 802.1Q is an industry standard protocol created by the IEEE (Institute of Electrical and Electronics Engineers). 
- You will probably NEVER use ISL in the real world. Even modern Cisco equipment doesn’t support it. For the CCNA, you only need to learn 802.1Q

![](packet.png)
  

- The 802.1Q tag is inserted between the Source and Type/Length fields of the Ethernet frame. 
- The tag is 4 bytes (32 bits) in length. 
![](802.1%20Q%20tag%20format.png)
- The tag consists of two main fields: Tag Protocol Identifier (TPID) Tag Control Information (TCI) 
- The TCI consists of three sub-fields

- **TPID (Tag Protocol Identifier)**
	- 16 bits (2 bytes) in length 
	- Always set to a value of 0x8100. This indicates that the frame is 802.1Q-tagged.
	- 0x means hexadecimal, the actual value in the field is just 8100.
	- For hexadecimal digits, each hexadecimal digit is four bits, so four times four is 16.
	- Since the dot one Q tag comes after the source mark field of the Ethernet frame. This is where the type field is usually located.
	- When the switch sees this value of 8100 here, it knows it's a .1Q tagged frame.

- **PCP (Priority Code Point)** 
	-  3 bits in length 
	- Used for Class of Service (CoS), which prioritizes important traffic in congested networks. 

  
- **DEI (Drop Eligible Indicator)**  
	- 1 bit in length 
	-  Used to indicate frames that can be dropped if the network is congested

  
**- VID (VLAN ID)**  
	- 12 bits in length 
	-  Identifies the VLAN the frame belongs to. 
	- 12 bits in length = 4096 total VLANs (2^12), range of 0 - 4095 
	-  VLANs 0 and 4095 are reserved and can’t be used. Therefore, the actual range of VLANs is 1 – 4094 
	- Cisco’s proprietary ISL also has a VLAN range of 1 - 4094

- Now let’s see the diagram again

![](trunk%20port_2.png)
- So this PC and VLAN 10 wants to send traffic to this other PC in Vlan 10.
- The traffic goes to SW2, which then forwards it to SW1 with a tag indicating that the traffic belongs to VLAN 10. 
- SW1 receives the frame and because the destination is also in Vlan 10, it will forward the traffic to the destination.
- Remember, a standard layer two switch like this will only forward traffic in the same VLAN, it will not forward traffic between VLANs.

#### Native VLAN  
- 802.1Q has a feature called the native VLAN. (ISL does not have this feature)
- The native VLAN is VLAN 1 by default on all trunk ports, however this can be manually configured on each trunk port. 
	- It's important to remember that this has to be configured on each trunk port separately.
	- It's not a global configuration on the switch.
- The switch does not add an 802.1Q tag to frames in the native VLAN. 
	- It will forward the frame normally without adding the dot one Q tag to it.
- When a switch receives an untagged frame on a trunk port, it assumes the frame belongs to the native VLAN. It’s very important that the native VLAN matches! 

- When native VLAN matches
![](when%20native%20VLAN%20matches.png)
- When native VLAN not match
![](when%20native%20VLAN%20not%20match.png)

- When native VLAN mismatch and frame is also tagged with one of the VLAN in native VLAN

![](When%20native%20VLAN%20mismatch%20and%20frame%20is%20also%20tagged%20with%20one%20of%20the%20VLAN%20in%20native%20VLAN.png)

#### Router on a Stick (ROAS)
- ROAS is used to route between multiple VLANs using a single interface on the router and switch.
- The switch interface is configured as a regular trunk.
- The router interface is configured using subinterfaces. You configure the VLAN tag and IP address on each subinterface.
- The router will behave as if frames arriving with a certain VLAN tag have arrived on the subinterface configured with that VLAN tag.
- The router will tag frames sent out of each subinterface with the VLAN tag configured on the subinterface.

![](ROAS.png)


---
---

VLAN (part 3)

- The native VLAN feature does have one benefit because frames in the native VLAN aren't tagged. It's more efficient. Each frame is smaller, so it allows the device to send more frames per second.
- There are 2 methods of configuring the native VLAN on a router:
	- Use the command encapsulation dot1q vlan-id native on the router subinterface
	   ![](method%201%20for%20native%20vlan%20on%20router.png)
	- Configure the IP address for the native VLAN on the router’s physical interface (the encapsulation dot1q vlan-id command is not necessary)
	   ![](method%202%20for%20native%20vlan%20on%20router.png)

#### Layer 3 (Multilayer) Switches
- A multilayer switch is capable of both switching AND routing.
- It is ‘Layer 3 aware’.
	- A regular layer two switch is not layer three aware.
	- It doesn't think at all about IP addresses or anything above layer two.
	- It only cares about layer two information like MAC addresses.
- You can assign IP addresses to its interfaces, like a router.
- You can create virtual interfaces for each VLAN, and assign IP addresses to those interfaces. 
- You can configure routes on it, just like a router.
- It can be used for inter-VLAN routing.
	- So far we have looked at two methods of inter VLAN routing.
	- The first one is using one connection for each VLAN between the router and switch.
		- This works, but if you have many VLANs, you probably won't have enough interfaces on your router.
	- The second method was router on a stick, which uses a single trunk connection which carries traffic from all VLANs between the switch and router for interview LAN routing.
		- This is efficient in terms of the number of interfaces.
		- Just one, but in a busy network.
		- All of the traffic going to the router and back to the switch can cause network congestion.
	- So in large networks, a multilayer switch is the preferred method of inter VLAN routing.	

#### Inter-VLAN Routing via SVI
- SVIs (Switch Virtual Interfaces) are the virtual interfaces you can assign IP addresses to in a multilayer switch.
- Configure each PC to use the SVI (NOT the router) as their gateway address.
- To send traffic to different subnets/VLANs, the PCs will send traffic to the switch, and the switch will route the traffic.

layer 3 switch's SVI
![](SVI.png)

- Now let's take a look at the path that traffic between these two PC's takes this time.
- The frame arrives at SW2, the destination is in the 192.168.1.0/26 subnet.
- SW2 now has its own routing table. So it looks up the destination in the routing table and sees that the destination is connected to its VLAN 10 SVI. So the traffic is now routed to VLAN 10.
	- If SW2 doesn't have the destination MAC address in its MAC address table, it will flood the frame to all VLAN 10 interfaces.
- it forwards it to SW1 over its trunk interface tagged as VLAN 10, SW1, then forwards it to the destination.

for traffic of outside network
![](for%20traffic%20of%20outside%20network.png)

- Now, what if the hosts want to reach destinations outside of the land?
- For example, I've added a cloud connected to R1 to represent the internet.
- Because SW2 is their default gateway. Any packets destined outside of their subnet will be sent to SW2.
- But our previous ROAS configurations for the connection between SW2 and R1 will no longer work.
- In addition to configuring virtual interfaces SVI on multilayer switches, we can also configure their physical interfaces to operate like a router interface rather than a switch port.
- So we can assign the subnet, 192.168.1.192/30 for this point to point link between SW2 and R1 with SW2 interface.
- Having an IP address of 192.168.1.193 and R1 g0/0 interface having an IP address of 192.168.1.194.
- Then we configure a default route on SW2, pointing toward R1. So all traffic destined outside of the LAN will be sent to R1.