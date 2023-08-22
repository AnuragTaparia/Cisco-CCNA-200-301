
STP part 1

-----


### Network Redundancy
- Redundancy is an essential part of network design.
- Modern networks are expected to run 24/7/365. Even a short downtime can be disastrous for a business.
- If one network component fails, you must ensure that other components will take over with little or no downtime.
- As much as possible, you must implement redundancy at every possible point in the network.
- Most PCs only have a single network interface card (NIC), so they can only be plugged into a single switch. However, important servers typically have multiple NICs, so they can be plugged into multiple switches for redundancy.

### Spanning Tree Protocol
- ‘Classic Spanning Tree Protocol’ is IEEE 802.1D.
- Switches from ALL vendors run STP by default.
- STP prevents Layer 2 loops by placing redundant ports in a blocking state, essentially disabling the interface.
- These interfaces act as backups that can enter a forwarding state if an active (=currently forwarding) interface fails.
- Interfaces in a forwarding state behave normally. They send and receive all normal traffic.
- Interfaces in a blocking state only send or receive STP messages (called BPDUs = Bridge Protocol Data Units).
- Broadcast Storm
	- To better understand the importance of STP and how STP prevents broadcast storms on a network with redundant links, consider the following example:
	- ![](broadcast%20storm.png)
	- SW1 sends a broadcast frame to SW2 and SW3. 
	- Both switches receive the frame and forward the frame out of every port except the port on which the frame was received. 
	- So SW2 forwards the frame to SW3. SW3 receives that frame and forwards it to SW1. SW1 then again forwards the frame to SW2! The same thing also happens in the opposite direction. 
	- Without STP in place, these frames would loop forever. STP prevents loops by placing one of the switch ports in a blocking state.

- With Spanning Tree Protocol (STP), our network topology above would look like this
	![](broadcast%20storm%20solve.png)
	- In the topology above, STP has placed one port on SW3 in the blocking state. That port will no longer process any frames except the STP messages. If SW3 receives a broadcast frame from SW1, it will not forward it out to the port connected to SW2.
	- Spanning Tree Protocol (STP) enables layer 2 redundancy. In the example above, if the link between SW3 and SW1 fails, STP will converge and unblock the port on SW3.

- By selecting which ports are forwarding and which ports are blocking, STP creates a single path to/from each point in the network. This prevents Layer 2 loops.    
- There is a set process that STP uses to determine which ports should be forwarding and which should be blocking. 
- STP-enabled switches send/receive Hello BPDUs out of all interfaces, the default timer is 2 seconds (the switch will send a Hello BPDU out of every interface, once every 2 seconds). 
- If a switch receives a Hello BPDU on an interface, it knows that interface is connected to another switch (routers, PCs, etc. do not use STP, so they do not send Hello BPDUs
- Switches use one field in the STP BPDU, the Bridge ID field, to elect a root bridge for the network. 
- The switch with the lowest Bridge ID becomes the root bridge. 
-  ALL ports on the root bridge are put in a forwarding state, and other switches in the topology must have a path to reach the root bridge
- Bridge ID
	![](Bridge%20id.png)
	![](bridge%20id%202.png)
	![](bridge%20id%203.png)
	![](Bridge%20id%204.png)
	- The STP bridge priority can only be changed in units of 4096. The valid values you can configure are: 0, 4096, 8192, 12288, 16384, 20480, 24576, 28672, 32768, 36864, 40960, 45056, 49152, 53248, 57344, or 61440. The Extended System ID will then be added to this number to make the total bridge priority
  
- All interfaces on the root bridge are designated ports. Designated ports are in a forwarding stat  
- When a switch is powered on, it assumes it is the root bridge. 
-  It will only give up its position if it receives a ‘superior’ BPDU (BPDU with lower bridge ID). 
-  Once the topology has converged and all switches agree on the root bridge, only the root bridge sends BPDUs. 
- Other switches in the network will forward these BPDUs, but will not generate their own original BPDUs  

- Steps of creating STPs
	- The switch with the lowest bridge ID is elected as the root bridge. All ports on the root bridge are designated ports (forwarding state).
	- Each remaining switch will select ONE of its interfaces to be its root port. The interface with the lowest root cost will be the root port. Root ports are also in a forwarding state.
	- Cost associate with interfaces!
		![](cost%20associate%20with%20interface.png)
		- The route cost is the total cost of the outgoing interfaces along the path to the route bridge.
		- you don't count the cost of the receiving interface, just the sending of the outgoing interface.
		- ![](cost%201.png)
		- ![](cost%202.png)
		- So SW1 advertises its route cost of 0 in its BPDU. SW2 will receive the BPDU and add the cost of its outgoing interface g01 which is for when it floods those BPDU out of its interfaces. SW3 will do the same.
		- So which port do you think SW2 will choose as its report?
		- Here is its logic.
		- It was advertised a cost of 0 on its g0/1 interface. However, the cost of its interface is 4 . Therefore, the total route cost via g0/1 is four. It was advertised at cost of four on g0/0 from SW3. However, its interface also has a cost of 4, so the total route cost via g0/0 is 8. So it will select g0/1 as the route port.
		- SW3 Logic follows the same process.
		- It has a total cost of four via g0/0 and a total cost of eight via g01. So it will select the g0/0 as its root port

- Let us review the points
-  One switch is elected as the root bridge. All ports on the root bridge are designated
   ports (forwarding state). Root bridge selection:
	- Lowest bridge ID
-  Each remaining switch will select ONE of its interfaces to be its root port (forwarding
   state). Ports across from the root port are always designated ports.
   Root port selection:
	- Lowest root cost
	- Lowest neighbor bridge ID
	- Lowest neighbor port ID (The NEIGHBOR switch’s port ID is used to break the tie, not the local switch’s port ID) (for example see ppt)
- Each remaining collision domain will select ONE interface to be a designated port
  (forwarding state). The other port in the collision domain will be non-designated
  (blocking)
  Designated port selection:
	-  Interface on switch with lowest root cost
	-  Interface on switch with lowest bridge ID

----

STP part 2

----

#### **Types of port states**

![](STP%20Port%20State.png)

 - Root/Designated ports remain stable in a Forwarding state. 
- Non-designated ports remain stable in a Blocking state. 
- As long as the network is stable, each spanning tree interface will be stable in one of these interfaces.
- Listening and Learning are transitional states which are passed through when an interface is activated, or when a Blocking port must transition to a Forwarding state due to a change in the network topology. 
- There is one more state called **Disabled** state. This simply refers to an interface that is administratively disabled, meaning shutdown. It doesn’t play any role in spanning tree, because the interface is shut down.
   
- **Blocking State**  
	- Non-designated ports are in a Blocking state. 
	- Interfaces in a Blocking state are effectively disabled to prevent loops. (This is what makes spanning tree work. Disabling redundant interfaces to avoid loops.)
	- Interfaces in a Blocking state do not send/receive regular network traffic. (Any regular traffic that arrives on an interface in a blocking state will simply be dropped.)
	-  Interfaces in a Blocking state receive STP BPDUs. (They need to receive and process BPD use to be aware of the spanning tree topology and be ready to transition towards a forwarding state if they need to.)
	- Interfaces in a Blocking state do NOT forward STP BPDUs. 
	-  Interfaces in a Blocking state do NOT learn MAC addresses. (If regular traffic arrives on the interface, it is dropped without adding the MAC address to the MAC address table )

- **Listening State**
	- After the Blocking state, interfaces with the Designated or Root role enter the Listening state. 
	- Only Designated or Root ports enter the Listening state (Non-designated ports are always Blocking) (That's because listening is a transitional state that eventually leads to the forwarding state.)
	-  The Listening state is 15 seconds long by default. This is determined by the Forward delay timer. 
	- An interface in the Listening state ONLY forwards/receives STP BPDUs.
	- An interface in the Listening state does NOT send/receive regular traffic. (If a regular unicast frame is received on a port in the listening state, it will be discarded)
	- An interface in the Listening state does NOT learn MAC addresses from regular traffic that arrives on the interface (when a frame arrives on a switch interface, the switch uses the source Mac address field to learn that Mac address and it updates the Mac address table with the Mac address interface and VLAN information. However, if an interface is in the spanning tree listening state, it will not do this. The traffic is simply dropped and the Mac address learning process does not occur.)
  
- **Learning State**  
	- After the Listening state, a Designated or Root port will enter the Learning state. 
	- The Learning state is 15 seconds long by default. This is determined by the Forward delay timer (the same timer is used for both the Listening and Learning states) 
	- An interface in the Learning state ONLY sends/receives STP BPDUs. 
	- An interface in the Learning state does NOT send/receive regular traffic. 
	-  An interface in the Learning state learns MAC addresses from regular traffic that arrives on the interface

- **Forwarding State**
	- Root and Designated ports are in a Forwarding state. 
	- A port in the Forwarding state operates as normal. 
	-  A port in the Forwarding state sends/receives BPDUs. 
	-  A port in the Forwarding state sends/receives normal traffic. 
	-  A port in the Forwarding state learns MAC addresses.

- **Here's a summary of each spanning port state**
![](spanning%20tree%20port%20state.png)

#### **Spanning Tree Timers**

![](STP%20timmer.png)

- Hello times
	- It determines how often the route bridge sends hello BPDU.
	- By default it will send them every 2 seconds.
	- Other switches in the network do not originate their own BPDU, but they will forward the BPDU they receive.
	- The switches will only forward BPDU on their designated ports.
	- Let's see how that works.
	- ![](hello%20timer%201.png)
    
	- Assuming these switches all come online at the same time, each assumes they are the route bridge and each will send BPD use out of all interfaces.
	- However, once the network has converged and all switches and ports are stabilised in their roles, only the root bridge sends BPDU.
	- Then the other switches will forward these BPDU on their designated ports. Updating information like the bridge route cost, sending bridge ID, sending port ID, etc..
	- ![](hello%20timer%202.png)
	- Then 2 seconds later, the route bridge will send the BPDU again and the other switches will again forward these BPDUs on their designated ports.
	- Switches do not forward the BPDUs out of their root ports and non-designated ports, only their designated ports.   

  
- **Max Age**
	- This timer indicates how long an interface will wait to change the Spanning tree topology after ceasing to receive BPDU.
	- The timer reset every time a BPDU is received
	- If another BPDU is received before the max age timer counts down to 0, the time will reset to 20 seconds and no changes will occur. 
	-  If another BPDU is not received, the max age timer counts down to 0 and the switch will reevaluate its STP choices, including root bridge, and local root, designated, and non-designated ports.
	- If a non-designated port is selected to become a designated or root port, it will transition from the blocking state to the listening state (15 seconds), learning state (15 seconds), and then finally the forwarding state. So, it can take a total of 50 seconds for a blocking interface to transition to forwarding. 
	- These timers and transitional states are to make sure that loops aren’t accidentally created by an interface moving to a forwarding state too soon. 

  
- A forwarding interface can move directly to a blocking state (there is no worry about creating a loop by blocking an interface).
- A blocking interface cannot move directly to forwarding state. It must go through the  listening and learning states.

- The STP timers on the root bridge determine the STP timers for the entire network
  

### **panning Tree Optional Features (STP Toolkit)**

- Portfast
	- Portfast allows a port to move immediately to the Forwarding state, bypassing Listening and Learning. 
	- If used, it must be enabled only on ports connected to end hosts. 
	- If enabled on a port connected to another switch it could cause a Layer 2 loop
	- Portfast will only have effect when the interface is in the non trunking mode (meaning access mode)
	- Port fast can also cause loops if the network cabling is changed without the proper caution
  
- BPDU Guard  
	- If an interface with BPDU Guard enabled receives a BPDU from another switch, the interface will be shut down to prevent a loop from forming
	- We can enable BPDU Guard on Portfast-enabled interfaces
  
- spanning tree load balancing.
	- If you have multiple VLANs in your network, blocking the same interface in each VLAN is a waste of interface bandwidth.
	- That connection will be doing nothing, just waiting for another connection to fail so it can start forwarding.
	- However, if you configure a different route bridge for different VLANs, different VLANs will disable different interfaces.
