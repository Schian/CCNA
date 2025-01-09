# Spanning Tree Protocol

## Part 1

The Spanning Tree Protocol allows for redundancy to be built into network without creating layer 2 loops, known as "Broadcast Storms". This allows the switches themselves to determine an efficient path through the network to a *"root switch"* (called a root bridge). Once a root switch has been designated it will broadcast BPDUs with each switch determining which interface will have the lowest *"root cost"*. When a switch is powered on, it will assume it is the root bridge and only give up its position if a superior BPDU is received.

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

---

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

## Part 2

- **STP States**
  - There are four Spanning Tree Port states (five, if you include 'disabled').
    - Root/Designated ports remain stable in a **forwarding** state.
    - Non-designated ports remain stable in a **blocking** state.
    - **Listening** and **Learning** are transitional states, stepped through when an interface is activated, or when a **Blocking** port must transition to a **Forwarding** state.
      - Usually due to a change in network topology.
  - **Blocking State**
    - Only receives STP BPDUs, all other traffic is dropped and nothing is forwarded.
      - In case the interface needs to transition to a forwarding state.
    - Does not learn MAC addresses
    - Effectively disabled to prevent loops (broadcast storms)
  - **Listening State**
    - Transitional state after an interface has changed from a non-designated to a designated or root role.
    - This state is 15 seconds long by default.
      - Set by the **Forward delay** timer.
    - Only forwards/receives BPDUs, all other traffic is dropped
    - Does not learn MAC addresses
    - Used to check-and-verify to ensure that bringing the interface online isn't immediately going to cause loops.
  - **Learning State**
    - Transitional state after the listening sate.
    - This state is 15 seconds long by default.
      - Set by the **Forward delay** timer.
    - Only forwards/receives BPDUs, all other traffic is dropped
    - Does learn MAC addresses and begins to build the MAC table
    - Used to begin building the MAC table before forwarding traffic, reducing the risk of loops.
  - **Forwarding State**
    - A forwarding port operates "as normal"
    - All traffic and BPDUs are received, forwarded and sent as required.
    - Learns MAC addresses.

| STP Port State | Send/Receive<br>BPDUs | Frame<br>forwarding<br>(regular traffic) | MAC address<br>learning | Stable/<br>Transitional |
|:--------------:|:---------------------:|:----------------------------------------:|:-----------------------:|:-----------------------:|
| Blocking       | NO/YES                | NO                                       | NO                      | Stable                  |
| Listening      | YES/YES               | NO                                       | NO                      | Transitional            |
| Learning       | YES/YES               | NO                                       | YES                     | Transitional            |
| Forwarding     | YES/YES               | YES                                      | YES                     | Stable                  |

- **STP Timers**
  - - STP timers are to ensure adequate time has passed before transitioning to a forwarding state to avoid accidentally creating loops.
    - A forwarding interface can move directly to a blocking state. It is not possible to create a loop.
    - A blocking interface must move through the transitional states.
  - The root bridge will send a BPDU **Hello** message every 2 seconds to maintain the integrity of the network topology.
  - An interface will wait the **Max Age** time since it last received a **Hello** BPDU.
    - This is reset each time a BPDU is received
    - Default duration is 10 x Hello (20 seconds)
  - The **Forward Delay** timer determines how long the switch will stay in each transitional state.
    - 15 seconds for Listening
    - 15 seconds for Learning
    - 30 seconds total to transition
      - +20 seconds if **Max Age** is included.

| STP Timer     | Purpose                                                                                                                                                       | Duration                   |
|:-------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------------:|
| Hello         | How often the root bridge sends Hello BPDUs                                                                                                                   | 2 seconds                  |
| Forward delay | How long the switch will stay in the Listening<br>and Learning states (each state is<br>15 seconds = total 30 seconds)                                        | 15 seconds                 |
| Max Age       | How long an interface will wait to change the<br>STP topology *after ceasing to receive Hello<br>BPDUs*. The timer is reset every time a BPDU<br>is received. | 20 seconds<br>(10 x Hello) |

- **STP Load Balancing**
  - With Cisco's PVST it is possible to set different primary and secondary switches for different VLANs. This can help to improve load balancing through the network.
  - When a switch has been set as primary for a VLAN, it will automatically change its STP priority to 24,576 or 4096 less than the highest priority (lowest value). Whichever is the lower.
  - Setting a switch to secondary will set the STP priority to 28,672 or 4096 more than the primary. Whichever is the lower.
  - See [Part 2 - Config](#part-2---config) for commands.

## STP Toolkit

At the time I am taking this course, Jeremy is updating it to reflect the CCNA 1.1 update. He has released some videos improving the STP part of the course but it isn't finalised at this point, as he is devoting more time to the new or changed topics.

### PortFast

This feature allows interfaces to bypass the listening and learning states and be set to a forwarding state. This is very useful for interfaces intended for end hosts as there is no risk of a loop being created.

- When a host connects to a switchport the port becomes up/up but cannot send/receive data yet.
  - Designated port but must take 30 seconds to transition through listening and learning states.
- Leads to a poor user experience.
  - "The internet doesn't work"
  - Wait is unnecessary as there is no risk of a layer 2 loop
- PortFast sets an interface to a forwarding state
  - Bypassing listening and learning
- If configuring for all access ports at global config, this will not affect trunk links
- By default, cisco switches use PortFast ***Edge***.
  - There is also PortFast Network, but this is not a CCNA topic
- Can be configured for trunk ports
  - Safe if connecting to a Router-on-a-Stick or Virtualisation server.
  - Can only be configured on a per-port basis.

### BPDU Guard & BPDU Filter

These features help to prevent unauthorised devices from accessing or altering the network topology. BPDU Guard will disable a port if it receives a BPDU (end-host has been unplugged and a switch plugged in) and BPDU Filter stops a port from sending or processing BPDUs.

- **BPDU Guard** acts a safeguard against an unintended change to the network topology.
  - Unauthorised switches being plugged into an end-host port
- A BPDU Guard-enabled port will place the interface into the **error-disabled** state.
  - In effect, disabling the port.
- Can be enabled globally or on a per-port basis.
  - *If enabled globally, it will only effect PortFast-enabled ports.*
- If a BPDU Guard-enabled port receives a BPDU frame, the switch will place the port into an **ErrDisable** state
  - To re-enable a port in an ErrDisabled state:
    - **first solve the underlying issue**
    - Reset the port `shutdown` then `no shutdown`, or
    - Enable **ErrDisable Recovery**
- **ErrDisable Recovery** is a feature to automatically re-enable an err-disabled port after a short period of time
  - Default time period is 300 seconds (5 minutes)

- **BPDU Filter** prevents a port from sending BPDUs
  - If enabled on a port-by-port basis, ***STP will effectively be disabled on the port***
    - Port will not send BPDUs
    - Port will ignore BPDUs
  - If enabled globally, it will be activated on ***all PortFast-enabled ports***
    - Disable on a port-by-port basis
    - The port will not send BPDUs
    - If the port receives a BPDU when BPDU filter has been globally enabled:
      - PortFast and BPDU filter will be disabled
      - Port will resume normal STP port actions

- **BPDU Guard** and **BPDU Filter** can be enabled on the same port at the same time:
  - If BPDU filter is enabled in *global config mode* and the port receives a BPDU:
    - BPDU Filter will be disabled
    - BPDU Guard will be triggered (the interface will be placed in errdisable)
  - If BPDU Filter is enabled in *interface config mode* and the port receives a BPDU:
    - The BPDU will be ignored
    - BPDU Guard will not be triggered
- Recommendations:
  - Enable PortFast and BPDU Guard in any preferred way.
  - Only enable BPDU Filter by default (in global config mode)
    - There must be a **VERY** good reason to enable it per-port.

### Root Guard

### Loop Guard

## Configuration

### Part 1 - Lab

- View Spanning Tree Information
  - `SW1#show spanning-tree`
  - `SW1#show spanning-tree vlan <vlan-ID>` - For a specific VLAN
  - `SW1#show spanning-tree detail` - Detailed information
  - `SW1#show spanning-tree summary` - Lists the STP state of each interface
  - `SW1#show spanning-tree interface <ID> detail`

### Part 2 - Config

- Configure the Spanning Tree Mode
  - `SW1(config)#spanning-tree mode <mode>`
    - One of: `mst`, `pvst`, `rapid-pvst`
- Set a switch as the primary root bridge
  - `SW1(config)#spanning-tree vlan <ID> root primary`
    - This is a shortcut for the following commands (can be seen in `running-config`):
      - `spanning-tree mode pvst`
      - `spanning-tree extend system-id`
      - `spanning-tree vlan <ID> priority 24576`
- Set a switch as the secondary root bridge
  - `SW1(config)#spanning-tree vlan <ID> root secondary`
    - This is a shortcut for the following commands (can be seen in `running-config`):
      - `spanning-tree mode pvst`
      - `spanning-tree extend system-id`
      - `spanning-tree vlan <ID> priority 28672`
- Configure STP port cost
  - `SW1(config-if)#spanning-tree vlan <ID> cost <value>`
- Configure STP port priority
  - `SW1(config-if)#spanning-tree vlan <ID> port-priority <value>`
    - Must be in increments of 32

### STP Toolkit - Config

- **PortFast**
  - Configure PortFast on an individual access port
    - `SW1(config-if)#spanning-tree portfast`
    - `SW1(config-if)#spanning-tree portfast disable`
  - Configure PortFast on *all access ports*
    - `SW1(config)#spanning-tree portfast default`
  - Configure PortFast on a trunk port
    - `SW1(config-if)#spanning-tree portfast trunk`

- **BPDU Guard**
  - Configure BPDU Guard on an individual access port
    - `SW1(config-if)#spanning-tree bpduguard enable`
    - `SW1(config-if)#spanning-tree portfast disable`
  - Configure BPDU Guard on *all PortFast enabled ports*
    - `SW1(config)#spanning-tree portfast bpduguard default`
  - Configure **ErrDisable**
    - `SW1(config)#errdisable recovery cause <cause>`
    - `SW1(config)#show errdisable recovery`

- **BPDU Filter**
  - Configure BPDU filter on an individual access port
    - `SW1(config-if)#spanning-tree bpdufilter enable`
    - `SW1(config-if)#spanning-tree bpdufilter disable`
  - Configure BPDU filter on *all PortFast enabled ports*
    - `SW1(config)#spanning-tree portfast bpdufilter default`
