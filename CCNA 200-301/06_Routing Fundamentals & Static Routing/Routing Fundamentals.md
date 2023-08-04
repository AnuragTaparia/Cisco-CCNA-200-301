#### **What is Routing?**
- Routing is the process that routers use to determine the path that IP packets should take over a network to reach their destination.
	- Routers store routes to all of their known destinations in a routing table.
	- When routers receive packets, they look in the routing table to find the best route to forward that packet.

- A route tells the router: to send a packet to destination X, you should send the packet to next-hop Y.   %%(next-hop = the next router in the path to the destination.)%%
	- or, if the destination is directly connected to the router, send the packet directly to the destination.
	- or, if the destination is the router’s own IP address, receive the packet for yourself (don’t forward it).

#### **Types of Routing**

1) **Dynamic Routing**: Routers use dynamic routing protocols (ie. OSPF) to share routing information with each other automatically and build their routing tables. 
2) **Static Routing**: A network engineer/admin manually configures routes on the router.

p![[example.png]]

![[automatic routes.png]]

- These Local and Connected Routes are neither static nor dynamic they are automatically created when the interface is configured 

![[connected and local route.png]]
- A **connected route** is a route to the network the interface is connected to.
	- R1 G0/2 IP = 192.168.1.1/24
	-  Network Address = 192.168.1.0/24
	-  It provides a route to all hosts in that network (ie. 192.168.1.10, 192.168.1.100, 192.168.1.232, etc.)
	-  R1 knows: “If I need to send a packet to any host in the 192.168.1.0/24 network, I should send it out of G0/2”.
- A local route is a route to the exact IP address configured on the interface.
	- A /32 netmask is used to specify the exact IP address of the interface.
		- /32 means all 32 bits are ‘fixed’, they can’t change.
	-  Even though R1’s G0/2 is configured as 192.168.1.1/24, the connected route is to 192.168.1.1/32.
	-  R1 knows: “If I receive a packet destined for this IP address, the message is for me”.
- If an interface is disabled the connected and local route for that interface won't appear in routing table.

-  These three lines are not routes. They mean the following:
	- 192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
		- In the routing table, there are two routes to subnets that fit within the 192.168.1.0/24 Class C network, with two different netmasks (/24 and /32).
	-  192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks
		- In the routing table, there are two routes to subnets that fit within the 192.168.12.0/24 Class C network, with two different netmasks (/24 and /32).
	-  192.168.13.0/24 is variably subnetted, 2 subnets, 2 masks
		- In the routing table, there are two routes to subnets that fit within the 192.168.13.0/24 Class C network, with two different netmasks (/24 and /32).

![[connected route.png]]

- A route matches a destination if the packet’s destination IP address is part of the network specified in the route.
	- ie. a packet to 192.168.1.60 is matched by a route to 192.168.1.0/24, but not by a route to 192.168.0.0/24.

![[local route.png]]

- When R1 receives a packet destined for 192.168.1.1, it will select the route to 192.168.1.1/32.
	- R1 will receive the packet for itself, rather than forward it out of G0/2.
	Local route = keep the packet, don’t forward

![[route selection.png]]

- If a router receives a packet and it has multiple routes that match the packet’s destination, it will use the **most specific** matching route to forward the packet.
	- Most specific matching route = the matching route with the longest prefix length.
	- This is different than switches, which look for an exact match in the MAC address table to forward frames

- unlike switches , which flood frames if they don't know where the destination is, routers do not
- Routers never floods "packets". If the router doesn't have a route to the packet's destination, it will drop the packet 

- ##### Now Let's see for R2 connected and local routes
- ![[R2.png]]
- ##### Now Let's see for R3 connected and local routes
- ![[R3.png]]
- ##### Now Let's see for R4 connected and local routes
- ![[R4.png]]


### **Routing Packets 

![[Routing Packet.png]]
-  **Default Gateway**
	- End hosts like PC1 and PC4 can send packets directly to destinations in their connected network.
		- PC1 is connected to 192.168.1.0/24, PC4 is connected to 192.168.4.0/24
	- To send packets to destinations outside of their local network, they must send the packets to their default gateway.
		- PC1 (Linux) Config:                                                PC4 (Linux) Config:
		 ![[PC1&4 Linux config.png]]
	- The default gateway configuration is also called a default route.
		- It is a route to 0.0.0.0/0 = all netmask bits set to 0. Includes all addresses from 0.0.0.0 to 255.255.255.255.
		- ![[default route def.png]]
	-  End hosts usually have no need for any more specific routes.
		- They just need to know: to send packets outside of my local network, I should send them to my default gateway.

- **Static Routes**
	- When R1 receives the frame from PC1, it will de-encapsulate it (remove L2 header/trailer) and look at the inside packet.
	- It will check the routing table for the most-specific matching route: ![[R1 route table.png]]
	- R1 has no matching routes in its routing table.
		- It will drop the packet.
	- To properly forward the packet, R1 needs a route to the destination network (192.168.4.0/24).
		- Routes are instructions: To send a packet to destinations in network 192.168.4.0/24, forward the packet to next hop Y.
	- There are two possible path packets from PC1 to PC4 can take:
		1) PC1 → R1 → R3 → R4 →PC4
		2) PC1 → R1 → R2 → R4 →PC4 

- **Static Route Configuration**
	- Each router in the path needs two routes: a route to 192.168.1.0/24 and a route to 192.168.4.0/24.
		- This ensures two-way reachability (PC1 can send packets to PC4, PC4 can send packets to PC1).
	- routers don’t need routes to all networks in the path to the destination.
		- R1 doesn’t need a route to 192.168.34.0/24.
		- R4 doesn’t need a route to 192.168.13.0/24.
	-  R1 already has a Connected route to 192.168.1.0/24. R4 already has a Connected route to 192.168.4.0/24.
		 - The other routes must be manually configured (using Static routes).
	- To allow PC1 and PC4 to communicate with each other over the network, let’s configure these Static routes on R1, R3, and R4.![[static routes.png]]
	- Static Route Configuration (R1)
	  ![[Static Route Configuration (R1).png]]
	- Static Route Configuration (R3) 
	  ![[Static Route Configuration (R3).png]]
	  - Static Route Configuration (R4)
	  ![[Static Route Configuration (R4).png]]
	- Static Route Configuration with exit-interface
	  ![[Static Route Configuration with exit-interface.png]]

- ### **Default Route**
  ![[default route.png]]
- A default route is a route to 0.0.0.0/0
	- 0.0.0.0/0 is the least specific route possible; it includes every possible destination IP address.
-  If the router doesn’t have any more specific routes that match a packet’s destination IP address, the router will forward the packet using the default route.
-  A default route is often used to direct traffic to the Internet.
	- More specific routes are used for destinations in the internal corporate network.
	- Traffic to destinations outside of the internal network is sent to the Internet.
 ![[default route configuration.png]]