# Dynamic Routing

- **Network Route**: A route to a network/subnet (mask length </32)
- **Host Route**: A route to a specific host (/32 mask)

## Overview

- Routers can use dynamic routing protocols to advertise information about the routes they know to other routers.
- They form '*adjacencies*' or '*neighbour relationships*' or '*neighbourships*' with adjacent routers to exchange this information
- If multiple routes to a destination are learned, the router determines which route is superior and adds it to the routing table.
  - The router uses the '*metric*' of the route to decide which is superior (lower metric = superior)

## Types of Dynamic Routing Protocols

There are two main categories of dynamic routing protocols.

- Exterior Gateway Protocol (EGP), and
- interior Gateway Protocol (IGP)

### Exterior Gateway Protocol (EGP)

EGP is used to share route *between* different *autonomous system* (AS), which is a single organisation (ie. a company). Border Gateway Protocol (**BGP**) is essentially the only EGP in widespread use and it uses the **Path Vector** algorithm.

EGP, BGP and Path Vector are not on the CCNA exam topics list and the lecture doesn't go into anymore detail. This is covered in CCNP Service Provider.

### interior Gateway Protocol (IGP)

IGP is used to share routes within a single. IGPs will use one of two algorithm types to share routes.

- **Distance Vector**, which is used by:
  - Routing Information Protocol (**RIP**)
  - Enhanced Interior Gateway Routing (**EIGRP**)
- **Link State**, which is used by:
  - Open Shortest Path First (**OSPF**)
  - Intermediate System to Intermediate System (**IS-IS**)

#### Distance Vector Routing Protocols

#### Link State Routing Protocols

## Dynamic Routing Protocol Metrics

## Administrative Distance

### Floating Static Routes
