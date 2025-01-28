# RIP & EIGRP

## RIP

- **Routing Information Protocol** (RIP)
- Industry standard protocol
- Distance Vector
- Metric: Hop count
  - Maximum hop count of 15
- Three versions
  - **RIPv1** and **RIPv2** for use with IPv4
  - **RIPng** (RIP Next Generation) for use with IPv6
- Two message types
  - **Request**: Ask RIP-enabled neighbour routers to send their routing table
  - **Response**: To send the local router's routing table to a neighbour router
- By default, routing table is shared every 30 seconds

### RIPv1 and RIPv2

- RIPv1
  - Only advertises *classful* addresses
  - Doesn't support VLSM or CIDR
  - Doesn't include subnet mask information in advertisements
  - Messages are broadcast to 255.255.255.255
- RIPv2
  - Supports VLSM and CIDR
  - Includes subnet mask information in advertisements
  - Messages are **multicast** to 244.0.0.9

#### `network` command

The **`network`** command tells the router to:

- Look for interfaces with an IP address that is in the specified range
- Activate RIP on the interfaces that fall in the range
- Form adjacencies with connected RIP neighbours
- Advertise the network prefix of the interface
- Not the prefix in the network command
- Examples at [RIP Config](#rip---config)
- The command is classful, so:
- The router will check its ouwn interfaces for IP addresses that match
  - `network 10.0.0.0` will enable both 10.10.0.0 and 10.0.12.0
- The router will advertise the network prefix of those interfaces
  - NOT the prefix of the address provided
  - 10.10.0.0/30 not 10.0.0.0/8

## EIGRP

## Configuration

### RIP - Config

- Show routing protocol information
  - `R1#show ip protocols`
- Enter router config mode
  - `R1(config)#router rip`
- Set the version
  - Always ensure v2 is used
  - `R1(config-router)#version 2`
- Turn off auto-summary
  - Stops the router from auto-resolving to classful addresses
  - `R1(config-router)#network 10.0.0.0`
  - `R1(config-router)#network 172.16.0.0`
  - **Note**: No netmasks
- Stop interfaces from sending advertisements
  - `R1(config-router)#passive-interface g2/0`
- Advertise the default route
  - `R1(config-router)#default-information originate`

### EIGRP - Config
