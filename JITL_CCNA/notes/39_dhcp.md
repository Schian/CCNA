# Dynamic Host Configuration Protocol

- DHCP allows hosts to automatically, or dynamically, learn various aspects of their network configuration.
- Typically used for client devices
  - Workstations, phones, etc
- Routers, servers, etc are usually manually configured
- Larger networks will use a DHCP server
  - Windows or Linux

## DHCP Four-Way Handshake

- **Discover**
  - Broadcast to MAC broadcast (all f's)
  - The `bootp` flags will be set to unicast or broadcast
    - To indicate the client's preference to the server
- **Offer**
  - Depending on the client's preference, unicast or broadcast back to the client
  - Provides an IP address to the client
  - Along with other information
    - Lease time, DNS server, etc
- **Request**
  - The client broadcasts its response back to the server
  - The `bootp` flags will be set to unicast or broadcast
    - To indicate the client's preference to the server
  - The client requests the offered IP address
- **Ack**
  - Depending on the client's preference, unicast or broadcast back to the client
  - The server allows the client to use that IP
    - The address is removed from the pool and assigned to the client's MAC address

|              |                  |                      |
|:-------------|:----------------:|:---------------------|
| **D**iscover | Client -> Server | Broadcast            |
| **O**ffer    | Server -> Client | Broadcast or Unicast |
| **R**equest  | Client -> Server | Broadcast            |
| **A**ck      | Server -> Client | Broadcast or Unicast |
|              |                  |                      |
| Release      | Client -> Server | Unicast              |

## DHCP Relay

- Large enterprises will use a centralised DHCP server
  - This server won't receive the DHCP client's broad messages
    - Broadcast messages don't leave the local subnet
- The router can act as a **DHCP relay agent**
  - The router will forward the client's DHCP messages to the remote DHCP server as unicast messages

## Configuration

- **DHCP Server Configuration**
  - Exclude addresses from the DHCP pool (inclusive)
    - `R1(config)#ip dhcp exclude-address <start ip> <end ip>`
  - Create a DHCP pool
    - `R1(config)#ip dhcp pool <name>`
  - Specify the subnet of addresses to be assigned
    - `R1(dhcp-config)#network <network ip> {netmask | prefix-length}`
  - Specify the DNS server
    - `R1(dhcp-config)#dns-server <ip>`
  - Specify the domain name of the network
    - `R1(dhcp-config)#domain-name <name>`
  - Specify the default gateway
    - `R1(dhcp-config)#default-router <ip>`
  - Specify the lease time
    - `R1(dhcp-config)#lease <days> <hours> <minutes>`
    - `R1(dhcp-config)#lease infinite`
  - Show leased addresses
    - `R1#show ip dhcp binding`
- **DHCP Relay**
  - Select the interface connected to the subnet
    - `R1(config)#interface <id>`
  - Configure the IP address of the DHCP server
    - There must be a route to the server
    - `R1(config-if)#ip helper-address <DHCP server ip address>`
- **DHCP Client**
  - Select the interface to receive an IP address from the DHCP server
    - `R1(config)#interface <id>`
  - Enable DHCP client
    - `R1(config-if)#ip address dhcp`
  - Enable the interface
    - `R1(config-if)#no shutdown`
