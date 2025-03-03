# WAN Architectures

## Lecture

- A WAN is a network that extends over a large geographic area
  - Used to connect geographically separate LANs
- The Internet itself can be considered a WAN
  - The term, 'WAN', is typically used to refer to an enterprise's private connections
    - To offices
    - To data centres
    - Other sites
- Over public or shared networks, like the Internet, VPNs can be used to create private WAN connections
- There have been many different WAN technologies over the years
  - Depending on the location, some will be available and some will not
  - Some technologies are considered 'legacy' in one country might still be used in others

### Leased Lines

- A **Leased line** is a dedicated physical link that will typically connect two sites
- Leased lines use serial connections (PPP or HDLC encapsulation)
- There are various standards that provide different speeds
  - Different standards are available in different countries
- Due to higher cost, higher installation lead time, and slower speeds of leased lines, Ethernet WAN technologies are becoming more popular

| ****             | **North American** | **European (CEPT)** |
|:----------------:|:------------------:|:-------------------:|
| **First Level**  | T1 - 1.544 Mbit/s  | E1 - 2.048 Mbit/s   |
| **Second Level** | T2 - 6.312 Mbit/s  | E2 - 8.448 Mbit/s   |
| **Third level**  | T3 - 44.736 Mbit/s | E3 - 34.368 Mbit/s  |

### MPLS

- **Multi Protocol Label Switching**
- Similar to the Internet, service providers' MPLS networks are shared infrastructure because many customer enterprises connect to and share the same infrastructure to make WAN connections
- The *label switching* in the name of MPLS allows VPNs to be created over the MPLS infrastructure through the use of **labels**
- Important terms:
  - **CE router**: Customer Edge router
  - **PE router**: Provider Edge router
  - **P router**: Provider core router
- When the PE routers receive frames from the CE routers, they add a label to the frame
- These labels are used to make forwarding decisions within the service provider network, **NOT** the destination IP
- Many different technologies can be used to connect to a service provider's MPLS network for WAN service

![MPLS](./images/mpls.png)

#### Layer 3 MPLS

- The CE routers to not use MPLS, it is only used by the PE and P routers
  - Provider Edge and Provider Core
- When using a *Layer 3 MPLS VPN* the CE and PE routers peer using a dynamic routing protocol (OSPF for example) to share routing information
- Example below
  - Office A's CE will peer with one PE
  - Office B's CE will peer with the other PE
  - Office A's CE will learn about Office B's routes via this OSPF peering, and
  - Office B's CE will learn about Office A's routes too

![Layer 3 MPLS](./images/layer3_mpls.png)

#### Layer 2 MPLS

- When using a *Layer 2 MPLS VPN*, the CE and PE routers do not form a peering
- The service provider network is entirely *transparent* to the CE router
  - In effect, it is like the two CE routers are directly connected
  - Their WAN interfaces will be on the same subnet
- If a dynamic routing protocol is used, the two CE routers will peer directly with each other

### Internet Connections

- There are countless ways for an enterprise to connect to the internet
  - Leased lines, MPLS VPNs, CATV, and DSL
- These days, for both enterprise and consumer internet access, fiber optic Ethernet connections are growing in popularity due to the high speeds they provide over long distances

#### Digital Subscriber Line (DSL) and Cable Internet

- **DSL** provides Internet connectivity to customers over phone lines
  - Can share the same phone line that is already installed in most homes
- A DSL modem is required to convert data into a format suitable to be sent over the phone lines
  - The modem might be a separate device
  - It might be incorporated in to the 'home router'
- **Cable Internet** provides Internet access via the same CATV (Cable Television) lines used for TV service
- Like DSL, a cable modem is required to convert data into a format suitable to be sent over the CATV cables
  - This can, again, be a separate device or built into the home router
- **Redundant Internet Connections**
  - **Single Homed**: 1 connection to 1 ISP
  - **Dual Homed**: 2 connections to 1 ISP
  - **Multihomed**: 1 connection to each of 2 ISPs
  - **Dual Multihomed**: 2 connections to each of 2 ISPs

### Internet VPNs

- Private WAN services (leased lines and MPLS) provide security because each of the customer's traffic is separated by using dedicated physical connections (leased line) or by MPLS tags
- When using the Internet as a WAN to connect sites together, there is no built-in security by default
  - VPNs are used to provide secure communication over the Internet
    - Site-to-Site VPNs using IPsec
    - Remote-Access VPNs using TLS

#### Site-to-Site VPNs (IPsec)

- A 'site-to-site' VPN is a VPN between two devices and is used to connect two sites together over the Internet
- A VPN 'tunnel' is created between the two devices by encapsulating the original IP packet with a VPN header and a new IP header
  - When using IPsec, the original packet is encrypted **BEFORE** being encapsulated with the new header
    - The client sends unencrypted data to the gateway
    - The gateway encrypts the data and adds a VPN header and a new IP header
    - The encrypted data is transmitted along the IPsec VPN 'tunnel'
    - The receiving router decrypts the data and forwards to the next hop
- In a 'site-to-site' VPN, a tunnel is formed only between two tunnel endpoints
  - Two routers connected to the Internet
- All other devices in each site don't need to create a VPN for themselves
  - They can send unencrypted data to their site's router, which will encrypt and forward
- There are some limitations to standard IPsec
  1. IPsec doesn't support broadcast or multicast traffic, only unicast
     - This means that routing protocols, such as OSPF, can't be used over the tunnels because they rely on multicast traffic
     - This can be solved with 'GRE over IPsec
  2. Configuring a full mesh of tunnels between many sites is a labour-intensive task
     - This can be solved with Cisco's DMVPN

#### Generic Routing Encapsulation

- **GRE** creates tunnels like IPsec, however is does not encrypt the original packet, so it is not secure
  - However, it has the advantage of being able to encapsulate a wide variety of Layer 3 protocols as well as broadcast and multicast traffic
- To get the flexibility of GRE with the security of IPsec
  - 'GRE over IPsec' can be used
    - The original packet will be encapsulated by a GRE header and a new IP header
    - The GRE packet will then be encrypted and encapsulated within an IPsec VPN header and a new IP header

#### Dynamic Multipoint VPN

- **DMVPN** is a Cisco developed solution that allows routers to dynamically create a full mesh of IPsec tunnels without having to manually configure every single tunnel.
  1. Configure IPsec tunnels to a hub site
  2. The hub router gives each router information about how to form an IPsec tunnel with the other routers
- DMVPN provides the configuration simplicity of 'hub-and-spoke' and the efficiency of direct spoke-to-spoke communication
  - Each router only needs one tunnel configured
    - To the hub
  - The 'spoke' routers can communicate directly without traffic passing through the hub

#### Remote-Access VPNs

- Site-to-site VPNs are used to make a point-to-point connection between two sites over the Internet
- Remote-access VPNs are used to allow end devices to access "the company's" internal resources securely over the internet
- These connections will typically use TLS (Transport Layer Security)
  - TLS is what provides security for HTTPS
  - TLS was formerly known as SSL (Secure Sockets Layer) and developed by Netscape
    - Later renamed to TLS when standardised by the IETF
- VPN client software is installed on end devices
  - These end devices then form secure tunnels to one of the company's routers/firewalls acting as a TLS server
  - This allows the end users to securely access resources on the company's internal network without being directly connected to the company network

### Site-to-Site vs Remote Access

1. First
    - Site-to-Site VPNs typically use IPsec
    - Remote Access VPNs typically use TLS
2. Second
    - Site-to-Site VPNs provide service to many devices within the sites they are connecting
    - Remote Access VPNs provide service to the one end device the VPN client software is installed on
3. Third
    - Site-to-Site VPNs are typically used to permanently connect two sites over the Internet
    - Remote Access VPNs are typically used to provide on-demand access for end devices that want to securely access company resources while connected to a network which is not secure

## Practical (GRE Configuration)
