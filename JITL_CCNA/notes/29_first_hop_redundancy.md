# First Hop Redundancy Protocols

## Cheat Sheet

| **FHRP** | **Terminology** | **Multicast IP**                     | **Virtual MAC**                             | **Cisco Proprietary?** |
|:--------:|:---------------:|:-------------------------------------|:--------------------------------------------|:----------------------:|
| HSRP     | Active/Standby  | v1: `224.0.0.2`<br>v2: `224.0.0.102` | v1: `0000.0c07.acXX`<br>v2:`0000.0c9f.fXXX` | Yes                    |
| VRRP     | Master/Backup   | 224.0.0.18                           | `0000.5e00.01XX`                            | No                     |
| GLBP     | AVG / AVF       | 224.0.0.102                          | `0007.b400.XXYY`                            | Yes                    |

## Summary

First Hop Redundancy Protocols (FHRP) are used to protect the default gateway used on a subnetwork by allowing two or more routers to provide backup for that address. In the even of failure of an active router, the backup router will take over the address, usually within a few seconds.

- A **virtual IP** is configured on the two routers, and a **virtual MAC** is generated for the virtual IP
  - Each FHRP uses a different format or the virtual
- An **active** router and a **standby** router are elected.
  - Different FHRPs use different terms
- End hosts in the network are configured to use the virtual IP as their default gateway
- The active router replies to ARP requests using the virtual MAC address, so traffic destined for other networks will be sent to it.
- If the active router fails, the standby becomes the next active routers
  - The new active router sends **gratuitous ARP** messages so that switches will update their MAC address tables
- If the old router comes back online, by default it won't take back its role as the active router
  - The old router becomes a standby router
  - Preemption can be configured, allowing the old active router to take back its old role

## HSRP

### Hot Standby Router Protocol

- Cisco proprietary
- An **active** and **standby** router are elected
- Multicast IPv4 address
  - v1: `224.0.0.2`
  - v2: `224.0.0.102`
- Virtual MAC address
  - v1: `0000.0c07.acXX`
    - `XX` is the HSRP group number
  - v2: `0000.0c9f.fXXX`
    - `XXX` is the HSRP group number
- Each subnet/VLAN can be configured with a different active router

## VRRP

### Virtual Router Redundancy Protocol

- Open standard
- A **master** and **backup** router are elected
- Multicast IPv4 address:
  - `224.0.0.18`
- Virtual MAC address:
  - `0000.5e00.01XX`
    - `XX` is the VRRP group number
- Each subnet/VLAN can be configured with a different active router

## GLBP

### Gateway Load Balancing Protocol

- Cisco proprietary
- Load balances among multiple routers **within a single subnet**
- An **AVG (Active Virtual Gateway)** is elected
- Up to four **AVFs (Active Virtual Forwarders)** are assigned by the AVG
  - The AVG itself can be an AVG
- Each AVF acts as teh default gateway for a portion of the hosts in the subnet
- Multicast IPv4 address:
  - `224.0.0.102`
- Virtual MAC address:
  - `0007.b400.XXYY`
    - `XX` is the GLBP group number
    - `YY` is the AVF number

## Configuration of HSRP

- Show the HSRP configuration
  - `R1#show standby`
- Set the version of HSRP
  - `R1(config-if)#standby version <1 or 2>`
- Enable HSRP on the interface
  - `R1(config-if)#standby <group number> ip <ip>`
- Set the router priority
  - `R1(config-if)#standby <group number> priority <0 - 255>`
- Enable preemption
  - `R1(config)#standby 1 preempt`
