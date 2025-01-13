# EtherChannel

## Problem to solve

If two switches are connected together with multiple links, all except one will be disabled by spanning tree. These other links will remain unused unless the active link fails and then one other link will take over as the designated port and start forwarding. EtherChannel solves this by grouping multiple, physical interfaces together to act as a single logical interface. Spanning Tree Protocol will treat this group as a single interface.

- **EtherChannel** is represented in network diagrams by drawing a circle around the interfaces grouped together
  - STP treats the group as a single interface
- Other names for an EtherChannel:
  - Port Channel
  - LAG (Link Aggregation Group)

## Load Balancing

- EtherChannel load balances based on **"flows"**
  - A flow is a communication between two nodes in the network
  - Frames in the same flow will be forwarded using the same physical interface
    - If frames in the same flow were forwarded using different interfaces, some frames may arrive at the destination out of order
- Inputs that can be used to calculate flow interface selection
  - Source MAC
  - Destination MAC
  - Source AND Destination MAC
  - Source IP
  - Destination IP
  - Source and Destination IP

## Configuration Settings

There are three methods of EtherChannel configuration. Regardless of which is chosen, all member interfaces must have matching configurations:

- Same duplex (full/half)
- Same speed
- Same switchport mode (access/trunk)
- Same allowed and native VLANs

If any of the above is different, that interface will be excluded from the EtherChannel

### PAgP (Port Aggregation Protocol)

- Cisco proprietary protocol
- Dynamically negotiates the creation and maintenance of the EtherChannel
  - Similar to Dynamic Trunking Protocol for trunks
- Allows up to 8 interfaces to form a single EtherChannel

When configuring PAgP the channel group number must the same for all member interfaces. However the channel group number does not have to be the same on the other switch, the channel group number is only for the local switch.

### LACP (Link Aggregation Control Protocol)

- Industry standard protocol (IEEE 802.3ad)
- Dynamically negotiates the creation and maintenance of the EtherChannel
  - Similar to Dynamic Trunking Protocol for trunks
- Allows up to 16 interfaces to form a single EtherChannel, but only 8 will be active.
  - The other 8 will be in standby mode until an active interface fails.

| **LACP Config** | **Active**   | **Passive**         |
|:---------------:|:------------:|:-------------------:|
| **Active**      | EtherChannel | EtherChannel        |
| **Passive**     | EtherChannel | *No EtherChannel* |

### Static EtherChannel

- A protocol isn't used to determine if an EtherChannel should be formed.
- Interfaces are statically configured to form an EtherChannel
- Not usually recommended
- When configured `on` no other EtherChannel configuration will work
  - `on` + `{desirable|active}` will not work

## Layer 3 EtherChannel

When an EtherChannel is configure

## Configuration

### Confusing Interchangeable Names

- `etherchannel` is used when displaying configurations
- `port-channel` is used when configuring the EtherChannel "as an interface"
- `channel-group` is used when configuring interfaces to be part of an EtherChannel

### Configuration commands

- Display configurations
  - `SW1#show etherchannel summary`
  - `SW1#show etherchannel port-channel`
  - `SW1#show etherchannel load-balance`
- Configure load balancing
  - `SW1(config)#port-channel load-balance <mode>`
- Configure interface to be part of an EtherChannel
  - `SW1(config-if)# channel-group <number> mode {desirable|auto|active|passive|on}`
    - PAgP: `auto` or `desirable`
    - LACP: `active` or `passive`
    - Static: `on`
- Manually configure the Negotiation Protocol
  - `SW1(config-if)#channel-protocol {lacp|pagp}`
- Configure the EtherChannel to use Layer 3
  - First, create the Etherchannel
    - `SW1(config)#int range g0/0-3` - Configure a range of interfaces for an EtherChannel
    - `SW1(config-if-range)#no switchport` - Configure all interfaces to be layer 3 interfaces
    - `SW1(config-if-range)#channel-group 1 mode active` - EtherChannel `1` created using LACP
  - Assign the EtherChannel an IP address
    - `SW1(config)#int port-channel1` - Enter interface configuration mode for the EtherChannel
    - `SW1(config-if)#ip address <IP> <netmask>` - Assign an IP address to the EtherChannel
