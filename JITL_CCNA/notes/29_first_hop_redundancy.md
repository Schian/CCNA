# First Hop Redundancy Protocols

## Cheat Sheet

| **FHRP** | **Terminology** | **Multicast IP**                     | **Virtual MAC**                             | **Cisco Proprietary?** |
|:--------:|:---------------:|:-------------------------------------|:--------------------------------------------|:----------------------:|
| HSRP     | Active/Standby  | v1: `224.0.0.2`<br>v2: `224.0.0.102` | v1: `0000.0c07.acXX`<br>v2:`0000.0c9f.fXXX` | Yes                    |
| VRRP     | Master/Backup   | 224.0.0.18                           | `0000.5e00.01XX`                            | No                     |
| GLBP     |                 |                                      |                                             |                        |

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

## HSRP (Hot Standby Router Protocol)
