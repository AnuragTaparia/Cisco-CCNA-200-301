
- A **computer network** is a digital telecommunications network which allows nodes to share resources.
- A **client** is device that accesses a service made available by a server.
- A **server** is a device that provides functions or services for clients.
- The same device can be client in some situations, and a server in other situations.
- Both servers and clients are end hosts in a network

- **Switches**:
	- have many network interfaces/ports for end hosts to connect to (usually 24+). 
	- provide connectivity to hosts within the same LAN (Local Area Network). 
	- do not provide connectivity between LANs/over the Internet
- **Routers**: 
	- have fewer network interfaces than switches. 
	- are used to provide connectivity between LANs. 
	- are therefore used to send data over the Internet

![switch and router connectivity](switch%20and%20router%20connectivity.png)


- **Firewalls**: 
	- monitor and control network traffic based on configured rules.
	- can be placed ‘inside’ the network, or ‘outside the network’. (meaning the firewall can filter the traffic before it reaches the router  or after it passes through the router)
	- are known as ‘Next-Generation Firewalls’ when they include more modern and advanced filtering capabilities.

Firewalls should be configured with security rules to determine which traffic should be allowed and which should be denied.

If you configured the rules properly , if PC in the new york branch tries to access server in tokyo, the firewall should permit the traffic through

![firewall 1](firewall%201.png)

the return traffic from server 1 to PC 1 should be allowed as well.

![firewall 2](firewall%202.png)

However, if the attacker tries to access anything inside of our networks, the firewall should block it

![firewall 3](firewall%203.png)

- The above firewall is a **network firewall**, they are hardware devices that filter traffic between networks.
- There are **host based firewall** which are software applications that filter traffic entering and exiting a host machine, like PC.

