# Rapid Spanning Tree Protocol

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
  - IEEE Standard
  - Uses modified RSTP mechanics
  - Can group multiple VLANs into different instances for load balancing
    - ie. VLANs 1-5 in instance 1, VLANs 2-10 in instance 2
