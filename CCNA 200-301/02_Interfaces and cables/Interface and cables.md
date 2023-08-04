### **Bit & Bytes**
- Speed is measured in bits per second (Kbps, Mbps, Gbps, etc), not bytes per second

![bits and bytes](bits%20and%20bytes.png)

![converting table](converting%20table.png)


### **Ethernet**
- Ethernet is a collection of network protocols/standards.
- Ethernet standards are Defined in the IEEE 802.3 standard in 1983.
	(IEEE = Institute of Electrical and Electronics Engineers)

![Ethernet standards (copper)](Ethernet%20standards%20(copper).png)


### **UTP Cables**
- UTP stands for Unshielded Twisted Pair
- Unshielded means that the wires have no metallic shield which can make them vulnerable to electrical interference
- The twist helps protect against electromagnetic interference or EMI
- there are 4 twisted pairs and 8 wires in total

![ethernet type and there cable wire](ethernet%20type%20and%20there%20cable%20wire.png)

- for 10BASE - T and 100BASE - T
	![pc to switch](pc%20to%20switch.png)
	
	![router to switch](router%20to%20switch.png)
	
	![switch to switch](switch%20to%20switch.png)
	
	![switch to switch via crossover cable](switch%20to%20switch%20via%20crossover%20cable.png)
	
	![router to pc via crossover cable](router%20to%20pc%20via%20crossover%20cable.png)
	
	![Device types and there Tx and Rx Pins table](Device%20types%20and%20there%20Tx%20and%20Rx%20Pins%20table.png)

- Newer networking devices have a feature called **Auto MDI-X**, it allows devices to automatically detect which pins their neighbor is transmitting data on and then adjust which pins they use to transmit and receive data 

- for 100BASE-T and 10GBASE-T
	- They use all 4 pairs (8 wires) to receive and transmit the data, Hence each pair is bidirectional
	![bidirectional](bidirectional.png)



### **Fiber-Optic Connections**
-  The copper UTP cables use separate wires within the cable to transmit and receive data, the fiber optic cables instead use separate cables to transmit and receive like this

![fibre optic connection](fibre%20optic%20connection.png)

- It uses SPF (small Form-Factor Pluggable) Transceiver to connect to router or switch
- These cables send light over glass fibre.
- There are four parts/layers in the cable: (from centre to the outer layer)
	1) **The fibreglass core**: Light is transmitted down this core to transmit data from one device to another.
	2) **Cladding** that reflects light
	3) **a protective buffer** which protects the fibreglass from breaking
	4) the outer jacket of the cable

![parts of fibre-optic cable](parts%20of%20fibre-optic%20cable.png)

- Types of Fibre-optic cables
	![types of fibre-optic cable](types%20of%20fibre-optic%20cable.png)
	- **Single mode fibre**: 
		- Core diameter is narrower than multimode fiber. 
		- Light enters at a single angle (mode) from a laser-based transmitter. 
		- Allows longer cables than both UTP and multimode fiber. 
		- More expensive than multimode fiber (due to more expensive laser-based SFP transmitters)
	- **Multi mode fibre**:
		- Core diameter is wider than single-mode fiber. 
		- Allows multiple angles (modes) of light waves to enter the fiberglass core. 
		- Allows longer cables than UTP, but shorter cables than single-mode fiber. 
		- Cheaper than single-mode fiber (due to cheaper LED-based SFP tranmitters).

![FibreOptics standards](FibreOptics%20standards.png)

![UTP vs FibreOptic](UTP%20vs%20FibreOptic.png)
