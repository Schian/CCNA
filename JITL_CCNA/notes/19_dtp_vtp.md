# DTP and VTP

**Note:** DTP and VTP have been removed from the CCNA exam topics list. However, knowing their function is important.

## Dynamic Trunking Protocol (DTP)

DTP is a Cisco propriety protocol to allow Cisco switches to dynamically determine their interface status without manual configuration. This is enabled by default, but for security purposes switchports should be manually configured and DTP disabled on all switchports.

- Configuring DTP to `desirable` will make that interface actively try to form a trunk.
- Configuring DTP to `auto` will make that interface create a trunk if the other end is a trunk.
- DTP will never form a trunk if the other end is set to `access`.

| **Administrative<br>Mode** | **Trunk** | **Dynamic<br>Desirable** | **Access** | **Dynamic<br>Auto** |
|:--------------------------:|:---------:|:------------------------:|:----------:|:-------------------:|
| **Trunk**                  | Trunk     | Trunk                    | Misconfig  | Trunk               |
| **Dynamic<br>Desirable**   | Trunk     | Trunk                    | Access     | Trunk               |
| **Access**                 | Misconfig | Access                   | Access     | Access              |
| **Dynamic<br>Auto**        | Trunk     | Trunk                    | Access     | Access              |

- Older switches have `switchport mode dynamic desirable` as the default administrative mode.
  - Beware of connecting these unconfigured to a network.
- Newer switches have `switchport mode dynamic auto` as the default administrative mode.
- Configuring `switchport mode access` will disable DTP
- ISL is favoured over 802.1Q if both are supported.
- DTP frames are sent over:
  - The native VLAN if 802.1Q
  - VLAN1 if ISL

## VLAN Trunking Protocol (VTP)

VTP allows for VLANs to be configured on a central switch, then propagated and synchronised through a network. This is desirable for a large networks with many VLANs so each switch doesn't have to be manually configured. However, VTP is rarely used and it is recommended not to be used. There are three VTP modes: server, client, and transparent.

- **VTP Server**
  - Can add/modify/delete VLANs
  - Store the VLAN database in non-volatile RAM (NVRAM)
  - Will increase the **revision number** with each VLAN modification
  - Will advertise the latest version of the VLAN database
  - **VTP servers also function as VTP clients**
    - A VTP server will synchronise to another VTP server with a higher revision number

- **VTP Clients**
  - Cannot add/modify/delete VLANs
  - Does not store the database
  - Will only synchronise to the database with the highest revision number
  - Will advertise their VLAN database
  - Will forward VTP advertisements to other clients

- **VTP Transparent**
  - Maintains its own VLAN database in NVRAM
    - Can add/modify/delete from this
  - Does not synchronise its VLAN database
  - Does not advertise its VLAN database
  - Will forward advertisements that are in the same domain as it.

- The revision number can be reset to 0 by:
  - Changing the VLAN domain to an unused domain
  - Changing the VTP mode to transparent

VTP has three versions. Version 1 is as described above and version 2 will support Token Ring VLANs. Version 3 won't be touched on as it is too far beyond the scope.

## Configuration

### DTP

- View DTP configuration
  - `SW1#show interfaces g0/0 switchport`
- Configure DTP
  - `SW1(config)#switchport mode dynamic desirable`
  - `SW1(config)#switchport mode dynamic auto`
- Disable DTP
  - `SW1(config)#switchport nonegotiate`

### VTP

- View VTP configuration
  - `SW1#show vtp status`
- Add to domain
  - `SW1(config)#vtp domain <name>`
- Set VTP Mode
  - `SW1(config)#vtp mode <client, server, transparent>`
