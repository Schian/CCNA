# Routing Fundamentals

## Part 1 - Fundamentals

- **Routing** is the process that routers use to determine the path that IP packets should take over a network to reach their destination
  - Routers store routes to all of their known destinations in a **routing table**.
  - When routers receive packets, they look in the routing table to find the best route to forward that packet.
- There are two main routing methods:
  - **Dynamic Routing**: Routers use *dynamic routing protocols* to share routing information with each other automatically and build their routing tables.
  - **Static Routing**: A network engineer/admin manually configures routes on the router.
- A **route** tells the router: *to send a packet to destination X, you should send the packet to **next-hop** Y.*
  - **OR**, if the destination is directly connected to the router, *send the packet directly to the destination.*
  - **OR**, if the destination is the router's own IP address, *receive the packet for yourself (don't forward).*

### Summary Slide

- Routers store information about destinations they know in their **routing table**.
  - When they receive packets, they look in the routing table to find the best route to forward the packet.
- Each **route** in the routing table is an instruction:
  - To reach destinations in network X, send the pack to **next-hop** Y.
  - If the destination is directly connected (**Connected** route) send the packet directly to the destination.
  - If the destination is the router's own IP address (**Local** route), receive the packet for yourself.
- When you configure an IP address on an interface and enable the interface, two routes are automatically added to the routing table:
  - **Connected** route (code **C** in the routing table)
    - A route to the network connected to the interface.
      - ie. if the interface's IP is **192.168.1.1/24**, the route will be to **192.168.1.0/24**
    - Tells the router: "To send a packet to a destination in this network, send it out of the interface specified in the route".
  - **Local** route (code **L** in the routing table)
    - A route to the exact IP address configured on the interface.
      - ie. if the interface's IP is **192.168.1.1/24**, the route will be to **192.168.1.1/32**.
      - Tells the router: "Packets to this destination are you. You should receive them yourself and not forward them"
- A route **matches** a destination if the packet's destination IP address is part of the network specified in the route.
  - ie. a packet to **192.168.1.60** is matched by a route to **192.168.1**.0/24, but not by a route to **192.168**.0.0/24
- If a router receives a packet and it doesn't have a route that matches the packet's destination, it will **drop** the packet.
  - This is different that switches, which **flood** frames if they don't have a MAC table entry for the destination.
- If a router receives a packet and it has multiple routes that match the packet's destination, it will use the **most specific** *matching route* to forward the packet.
  - **Most specific** matching route = the matching route with the **longest prefix length**.
  - This is different than switches, which look for an **exact** match in the MAC address table to forward frames.

## Part 2 - Static Routing

- **Static Routes** are set by the engineer/admin to send packets to the next hop.
  - Will have **code S** in the routing table.
  - When a router receives a packet, it will be de-encapsulated (remove L2 header/trailer) and the router will inspect the L3 header.
  - The router checks the routing table and does one of two things:
    - Forwards to the most-specific matching route.
    - Drop the packet if there is not a route specific enough
- **Default Gateway** are set on end hosts (Clients, servers, etc) allowing them to access destinations outside their own network.
  - Also called the **default route** and **Gateway of last resort**.
  - Will have __code S*__ in the routing table
  - All unknown destinations are forwarded to 0.0.0.0/0
    - This is the least specific matching route and everything is forwarded to this instead of being dropped.
  - Often used to direct traffic to the internet

### Configuration

- Each static route configured requires the destination and next hop.
- For **two-way reachability** each planned route requires two static routes set
  - One each way
- **Commands**
  - Setting a static route
    - `R1(config)# ip route <IP address> <netmask> <next hop IP>`
    - `R1(config)# ip route <IP address> <netmask> <exit interface>`
    - `R1(config)# ip route <IP address> <netmask> <exit interface> <next hop ip`
  - Setting the default
    - `R1(config)# ip route 0.0.0.0 0.0.0.0 <next hop IP>`
