# VLANS

## Part 1

A LAN is a single **broadcast domain**, including all devices in that broadcast . A **broadcast domain** is the group of devices which will receive a broadcast frame (destination MAC `FFFF:FFFF:FFFF:FFFF`) sent by any one of the members.

A **VLAN** logically separates a **broadcast domain** into multiple. This provides two main benefits:

1. **Performance**: Lots of unnecessary broadcast traffic can reduce network performance.
2. **Security**: You want to limit who has access to what.

Configuring a subnet will separate a network at layer 3, which will not stop a switch from flooding an unknown frame since this occurs at layer 2. Every device connected to the switch will receive the unknown frame.

By configuring VLANs to separate out the network, this will create separate broadcast domains. If a switch were to receive a broadcast frame, it would only flood the VLAN it received that frame on.

If an end host were to send a frame to another device connected to the switch but on a different VLAN. The sending end host would address this to its default gateway and not the final intended recipient. A switch does not perform **inter-VLAN routing**.

## Part 2

**Trunk Ports** are used to carry traffic from multiple VLANs over a single interface. This reduces the number of separate interfaces required when creating VLANs, which is advantageous for routers which only have a small number of interfaces.

Switches will 'tag' all frames that they send over a trunk link allowing the receiving switch to know which VLAN the frame belongs to. **Trunk Ports** are also known as 'tagged' ports and **Access Ports** are also known as 'untagged' ports.

The industry standard for trunking is `IEEE 802.1Q` also known as *`dot1q`* though there is an older, propriety standard Inter-Switch Link (ISL) but this is almost entirely phased out. The `dot1q` tag is inserted before the `Type/Length` field in the Ethernet header.

The `dot1q` tag is 4-bytes in length and consists of the Tag Protocol Identifier (`TPID`) field and the Tag Control Information (`TCI`) field. The `TCI` field contains three subfields, Priority Code Point (`PCP`), Drop Eligible Indicator (`DEI`), VLAN ID (`VID`).

| **Field/Sub-Field**                  | **Length**        | **Purpose**                                                                                |
|--------------------------------------|-------------------|--------------------------------------------------------------------------------------------|
| Tag Protocol Identifier (**`TPID`**) | 16 bits (2-bytes) | This is always set to `0x8100` indicating<br>the frame is `802.1Q`-tagged.                 |
| Tag Control Information (**`TCI`**)  | 16 bits (2-bytes) | Contains three sub-fields with<br>information about the frame.                             |
| Priority Code Point (**`PCP`**)      | 3 bits            | Used for Class of Service (CoS) to prioritise<br>important traffic in congested networks. |
| Drop Eligible Indicator (**`DEI`**) | 1 bit             | Indicates if the frame can be dropped.                                                     |
| VLAN ID (**`VID`**)                  | 12 bits           | Indicates the VLAN the frame belongs to.                                                   |

- The range of VLANs (1 - 4094) are divided into to sections:
  - Normal VLANs: 1 - 1005
  - Extended VLANs: 1006 - 4094
- Some older devices cannot use the extended range

The **Native VLAN**, defaulted to VLAN 1, will not be `802.1Q` tagged by a switch. The **Native VLAN** can be changed and it is encouraged to set this to an unused VLAN for security purposes, but it must be set the same on all switches have the same **Native VLAN** set. Otherwise frames will be unable to reach their intended host. **It is very important that the Native VLAN matches!**

**Router on a Stick** `ROAS` is used by a router to route between multiple VLANs using a single interface on the router and connected switch. The switch is configured as a regular trunk, but the router's interface is configured using **subinterfaces** ([See Configure Interface - Router](#part-2---config))

## Part 3

- **Native VLAN on a Router (ROAS)**
  - If you need or want to use the native VLAN for efficiency or performance reasons (at the expense of security), the router will need to be configured.
  - There are two methods to configure this. ([See Configure VLAN on a Router (ROAS)](#part-3---config))

- **Multilayer Switches**
  - Capable of both switching and routing making it "layer 3 aware"
  - IP addresses can be assigned  to its interfaces, like a router
  - Virtual interfaces can be created for each VLAN (and IP addresses assigned to those interfaces)
  - Routes can be configured
  - Can be used for inter-VLAN routing
    - Removing this burden from the network router
- **Switch Virtual Interfaces (SVI)**
  - Virtual interfaces on a multilayer switch
  - Each PC must be configured to use the SVI as its default gateway **(NOT THE ROUTER)**
  - The switch will handle the inter-VLAN routing
  - The switch will route internet and unknown traffic to the router
    - via default gateway
- **Conditions for Inter-VLAN Routing via SVI**
  - The VLAN must exist on the switch
  - The switch must have at least one access port in the VLAN OR on trunk port that allows the VLAN that is in an up/up state
  - The VLAN must not be shutdown
  - The SVI must not be shutdown (SVIs are disabled by default)

## Configuration

### Part 1 - Config

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

### Part 2 - Config

- Display configured trunks
  - `SW1#show interfaces trunk`
- Configure interfaces - **Switch**
  - `SW1(config-if)#switchport trunk encapsulation dot1q` - not always needed on modern switches
  - `SW1(config-if)#switchport mode trunk`
- Setting, adding, and removing VLANs from a trunk
  - `SW1(config-if)#switchport trunk allowed vlan <VLAN IDs>`         - Sets the trunk to allow the list of VLAN IDs
  - `SW1(config-if)#switchport trunk allowed vlan add <VLAN IDs>`     - Adds the VLAN IDs to the trunk
  - `SW1(config-if)#switchport trunk allowed vlan remove <VLAN IDs>`  - Removes the VLAN IDs from the trunk
- Set the **Native VLAN**
  - `SW1(config-if)#switchport trunk native vlan <VLAN ID>`
    - Best practice is to set this to an unused VLAN
    - **MUST BE THE SAME VLAN ON ALL SWITCHES**
- Configure interface - **Router**
  - `R1(config)#int g0/0`             - Enter the interface config mode
  - `R1(config-if)#no shutdown`       - Set to no shutdown before continuing
  - `R1(config-if)#interface g0/0.10` - Creates a subinterface
  - `R1(config-subif)#encapsulation dot1q <VLAN ID>`
  - `R1(config-subif)#ip address <IP> <netmask>`
    - The subinterface number and the VLAN number don't have to match, but it is best practice so as to make it easier to understand.

### Part 3 - Config

- Configure VLAN on a Router (ROAS).
  - Method 1, configure the subinterface:
    - `R1(config)#int g0/0.10`
    - `R1(config-subif)#encapsulation dot1q <VLAN ID> native`
  - Method 2, Configure the IP address for the native VLAN on the router's physical interface:
    - `R1(config)#int g0/0`
    - `R1(config-int)#ip address <IP in subnet> <subnet mask>`
    - *Specifying the encapsulation isn't necessary*
- Configure Inter-VLAN routing via SVI
  - Configure the default route
    - `SW1(config)#ip routing`                          - Enables Layer 3 routing on the switch
    - `SW1(config)#interface g0/0`                      - Enter the interface config mode
    - `SW1(config-if)#no switchport`                    - Configures this port as a 'routed port'. Layer 3 not layer 2 port
    - `SW1(config-if)#ip address <IP> <subnet>`         - In the example this is a /30, for the point-to-point connection to the router
    - `SW1(config-if)#exit`                             - Return to global config mode
    - `SW1(config)#ip route 0.0.0.0 0.0.0.0 <next hop>` - Set the default route
  - Configure SVIs
    - `SW1(config)#interface vlan <number>`             - Enter the SVI configuration
    - `SW1(config-if)#ip address <IP> <subnet>`         - Assign the SVI the IP of the gateway/default route for the VLAN
    - `SW1(config-if)#no shutdown`                      - Brings the SVI to an up/up state
