# Spanning Tree Protocol

## Part 1

The Spanning Tree Protocol allows for redundancy to be built into network without creating layer 2 loops, known as "Broadcast Storms". This allows the switches themselves to determine an efficient path through the network to a *"root switch"* (called a root bridge). Once a root switch has been designated it will broadcast BPDUs with each switch determining which interface will have the lowest *"root cost"*. When a switch is powered on, it will assume it is the root bridge and only give up its position if a superior BPDU is received.

- Spanning Tree Protocol (SPT)
  - **IEEE 802.1D**
  - Switches from all vendors run STP by default
- Builds redundancy without creating "Broadcast Storms"
  - Layer 2 broadcast loops
  - Places redundant ports into a blocking state
    - Act as the backup
- Interfaces in a forwarding state behave normally
- Interfaces in a blocking state only send/receive STP messages
  - BPDU

- **BPDUs** - Bridge Protocol Data Units
  - Used to determine which ports should be forwarding and which should be blocking
- STP-enabled switches send/receive Hello BPDUs out of all interfaces
  - Default timer is 2 seconds
  - If a BPDU is received, the switch knows the other device is a switch
    - Routers, PCs, etc. Do no use STP
- Switches elect the **Root Bridge** using the **Bridge ID** field in the **BPDU**
  - The switch with the **lowest Bridge ID** becomes the Root Bridge
  - All ports on the root bridge are put in a forwarding state, and other switches in the topology must have a path to reach the root bridge

1. **Bridge ID Structure**
   1. **Bridge Priority**. 16 bits.
      1. **Bridge Priority**. 4 bits.
      2. **Extended System ID (=VLAN ID)**. 12 bits.
   2. **MAC Address**. 48 bits.

- The default bridge priority is 32,768
  - 2^15 == 32,768 (the 16th and most significant bit)
  - This makes the MAC address as a "default tie-breaker"
    - Lowest MAC becomes root
- **Per-VLAN Spanning Tree** (PVST)
  - Allows a separate instance of STP for each VLAN
    - Gives each VLAN to have different interfaces forwarding/blocking
  - The VLAN ID is added to the priority (see **Bridge ID Structure** above)
    - Default VLAN 1 has a default priority of **32769**
      - 32,768 + 1
    - Changing the switches bridge priority can only be increased/decreased by 4096
      - The lowest value in the **Bridge Priority** subfield

- When powered on a switch assumes it is the root bridge
- When a 'superior' BPDU is received (lower bridge ID) it relinquishes its position
- Once the topology converges and all switches agree on the root bridge only it will send BPDUs
- The other switches will forward these BPDUs, but not generate their own

The non-root switches will select ONE of its interfaces to be its **root port**. The interface wil the lowest *root cost* will be the root port. Root ports are in a forwarding start

| **Speed** | **STP Cost** |
|:---------:|:------------:|
| 10 Mbps   | 100          |
| 100 Mbps  | 19           |
| 1 Gbps    | 4            |
| 10 Gbps   | 2            |

All collision domains must have a designated port and these are selected by using the switch with the lowest root cost, then the lowest bridge ID. The other switch will make its port non-designated (blocking)

### Summary - Part 1

1. One switch is elected as a root bridge. All ports on the root bridge are **designated ports** (forwarding state).
   1. The root bridge is selected by the lowest bridge ID
2. Each remaining switch will select **ONE** of its interfaces to be its **root port** (forwarding state).
   1. Ports across from (or connected to) the root port are always **designated** ports
   2. Root port selection
      1. Lowest root cost
      2. Lowest neighbour bridge ID
      3. Lowest neighbour port ID (if two switches have two connections to each other)
         1. The **NEIGHBOUR** port ID, not the local port ID
3. Each remaining collision domain will select ONE interface to be a **designated port** (forwarding state).
   1. The other port in the collision domain will be **non-designated** (blocking).
   2. Designated port selection:
      1. Interface on switch with lowest root cost
      2. Interface on switch with lowest bridge ID
