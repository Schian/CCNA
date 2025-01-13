# Rapid Spanning Tree Protocol (RSTP)

RSTP is not a timer-based spanning tree algorithm like 802.1D (the original spanning tree protocol). Therefore, RSTP offers an improvement over the 30 second or more that 802.1D takes to move a link to forwarding. The heart of the protocol is a new bridge-to-bridge handshake mechanism, which allows ports to move directly to forwarding.

## Spanning Tree Versions

- **Rapid Spanning Tree Protocol (802.1w)**
  - IEEE Standard
  - Much faster at converging/adapting to network changes than 802.1D
  - All VLANs share one STP instance
  - Cannot load balance
- **Rapid Per-VLAN Spanning Tree Plus (Rapid PVST+)**
  - Cisco Version of 802.1w
  - Each VLAN has its own STP instance
  - Can load balance by blocking different ports in each VLAN
- **Multiple Spanning Tree Protocol (802.1s)**
  - IEEE Standard and used by Cisco devices
  - Uses modified RSTP mechanics
  - Can group multiple VLANs into different instances for load balancing
    - ie. VLANs 1-5 in instance 1, VLANs 2-10 in instance 2
  - Best for large networks
  - Not a CCNA topic

## RSTP vs STP

- **Similarities**
  - RSTP serves the same purpose, blocking specific ports to prevent layer 2 loops
  - RSTP uses the same rules as STP:
    - To elect the root bridge
    - To elect root ports
    - To elect designated ports
- **Differences**
  - The port cost for determining a root cost have changed ([see RSTP cost table](#costs))
  - The port states have been simplified to just three states ([see RSTP states table](#states))
    - Discarding, Learning, Forwarding
      - Discarding is a combination of Blocking, Listening and Disabled
  - Port Roles
    - The r**oot port** role remains unchanged
      - The port "closest" to the root bridge becomes the root port
      - The root bridge does not have a root port
    - The **designated port** role remains unchanged
      - The port on a segment (collision domain) that sends the best BPDU is that segment's designated port
    - The **non-designated port** role is split into two separate roles ([see New Port Roles](#new-port-roles))
      - The **alternate port** role
      - The **backup port** role

### RSTP differences tables

### Costs

| **Speed** | **STP Cost** | **RSTP Cost** |
|:---------:|:------------:|:-------------:|
| 10 Mbps   | 100          | 2,000,000     |
| 100 Mbps  | 19           | 200,000       |
| 1 Gbps    | 4            | 20,000        |
| 10 Gbps   | 2            | 2000          |
| 100 Gbps  | undefined    | 200           |
| 1 Tbps    | undefined    | 20            |

### States

| **STP Port State** | **Send/Receive<br>BPDUs** | **Frame<br>Forwarding<br>(regular traffic)** | **MAC address<br>Learning** | **Stable/<br>Transitional** |
|:------------------:|:-------------------------:|:--------------------------------------------:|:---------------------------:|:---------------------------:|
| Discarding         | NO/YES                    | NO                                           | NO                          | Stable                      |
| Learning           | YES/YES                   | NO                                           | YES                         | Transitional                |
| Forwarding         | YES/YES                   | YES                                          | YES                         | Stable                      |

### New Port Roles

- **Alternate Port** role
  - A discarding port that receives a superior BPDU from another switch
    - A blocking port
  - Functions as a "backup" to the root port
    - If the root port fails, the switch changes the state of its best **alternate port** to forwarding.
    - This immediate move to the forwarding state functions like the classic STP optional feature UplinkFast.
      - Built into RSTP, does not need to be activated
- **Backup Port** role
  - A discarding port that receives a superior BPDU from *another interface on the same switch*
    - This can only happen when two interfaces are connected to the same collision domain, via a hub
    - Since hubs are not used in modern networks, encountering RSTP backup is unlikely
  - Function as a backup for a **designated port**
  - The interface with the lowest port ID will be selected as the designated port, the other will be the backup

### BPDU Differences

- Spanning Tree Protocol Version Numbers
  - 0 is the Protocol Version and BPDU Type for Classic STP
  - 2 is the Protocol Version and BPDU Type for Rapid STP
- All switches running Rapid STP:
  - Send their own BPDUs every `BPDU hello` time (2 seconds)
  - Switches 'age' the BPDU faster
    - A switch considers its neighbour lost if it misses 3 BPDUs (6 seconds)
    - The switch will flush all MAC addresses learned from that interface

### Link Types

- **Edge**
  - A port connected to an end host
  - Moves directly to forwarding, without negotiation
- **Point-to-Point**
  - A direct connection between two switches
- **Shared**
  - A connection to a hub
  - Must operate in half-duplex mode

## Configuration

- Configure the link type for the interface
  - Edge:   `SW1(config-if)#spanning-tree portfast` (not a typo)
  - Point-to-Point: `SW1(config-if)#spanning-tree link-type point-to-point`
  - Shared: `SW1(config-if)#spanning-tree link-type shared`
