# VLANS

## Part 1

A LAN is a single **broadcast domain**, including all devices in that broadcast . A **broadcast domain** is the group of devices which will receive a broadcast frame (destination MAC `FFFF:FFFF:FFFF:FFFF`) sent by any one of the members.

A **VLAN** logically separates a **broadcast domain** into multiple. This provides two main benefits:

1. **Performance**: Lots of unnecessary broadcast traffic can reduce network performance.
2. **Security**: You want to limit who has access to what.

Configuring a subnet will separate a network at layer 3, which will not stop a switch from flooding an unknown frame since this occurs at layer 2. Every device connected to the switch will receive the unknown frame.

By configuring VLANs to separate out the network, this will create separate broadcast domains. If a switch were to receive a broadcast frame, it would only flood the VLAN it received that frame on.

If an end host were to send a frame to another device connected to the switch but on a different VLAN. The sending end host would address this to its default gateway and not the final intended recipient. A switch does not perform **inter-VLAN routing**.

## Configuration

- Display configured VLANs
  - `SW1#show vlan brief`
- Create and name a VLAN
  - `SW1(config)#vlan 10`
  - `SW1(config-vlan)#name ENGINEERING`
- Configure VLANs - interfaces g1/0, g1/1, g1/2, g1/3
  - `SW1(config)#interface range g1/0 - 3`
  - `SW1(config-if-range)#switchport mode access`
  - `SW1(config-if-range)#switchport access vlan 10`
  - This sets these interfaces to vlan 10 as access ports
    - Instead of trunk ports
