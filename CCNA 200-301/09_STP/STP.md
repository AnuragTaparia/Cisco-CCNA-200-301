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