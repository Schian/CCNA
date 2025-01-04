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

## Configuration

### DTP

- View DTP configuration
  - `SW1#show interfaces g0/0 switchport`
- Configure DTP
  - `SW1(config)#switchport mode dynamic desirable`
  - `SW1(config)#switchport mode dynamic auto`
- Disable DTP
  - `SW1(config)#switchport nonegotiate`
