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

- Distance vector protocols were created before link state protocols in the 80s
  - Early examples are RIPv1 and Cisco's proprietary IGRP (which was updated to EIGRP)
- Distance vector protocols operate by sending the following to their directly connected neighbours:
  - Their known destination networks
  - Their metric to reach their known destination networks
- Known as "routing by rumour"
  - Each router doesn't know about networks beyond its neighbours
  - Only knows the information its neighbours tell it
- Called 'distance vector' because the routers only learn the following of each route:
  - The distance (metric), and
  - The vector (direction, the next-hop route)
- The metric is the number of hops
  - The speed of each link doesn't have an effect

#### Link State Routing Protocols

- When using a link state, each router creates a 'connectivity map' of the network
  - Each router advertises information about its interfaces and connected networks to its neighbours
  - This information is passed along to other routers until all routers in the network develop the same map of the network
- Each router independently uses this map to calculate the best routes to each destination
  - Link state protocols require more router CPU to process the extra shared information
  - But tend to be faster in reacting to changes in the network than distance vector protocols

## Dynamic Routing Protocol Metrics

A router's route table will contain the best route to each destination network it knows about. If the router is using a dynamic protocol learns two different routes to the same destination, it will use the **metric** value of the route to determine which is best. In this case a lower metric is better, and each routing protocol uses a different metric to determine which route is best. Since each metric is different, it can only be used to determine the best **route of the same protocol**. If there is more than one route to a destination with the same metric, both will be added to the routing table and traffic will be load balanced between them using **Equal Cost Multi-Path (ECMP)** load-balancing.

| **IGP** | **Metric**                                       | **Description**                                                                                                                                                                            |
|:-------:|:------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RIP     | Hop Count                                        | Each router in the path counts as one 'hop'. The total<br>metric is the total number of hops to the destination.<br>**Link speed doesn't affect cost. All links are equal**                |
| EIGRP   | Metric based on bandwith<br>& delay (by default) | Complex formula that can take into account many<br>values. By default, the bandwidth of the **slowest link<br>in the same route and the total delay of all links in the route<br>are used. |
| OSPF    | Cost                                             | The cost of each link is calculated based on bandwidth.<br>The total metric is the total cost of each link to the<br>route.                                                                |
| IS-IS   | Cost                                             | The total metric is the total cost of each link in the<br>route. The cose of each link is **not** automatically<br>calculated by default. All links have a cost of 10<br>by default.       |

## Administrative Distance

In most cases a company will oly use a single IGP - usually OSPF or EIGRP. However, in some rare cases they might use two. Since the metric is used to compare routes **learned via the same routing protocol** and different protocols use different metrics, they cannot be compared. This is where the **Administrative Distance** (**AD**) is used to compare which routing protocol is preferred. A lower AD is preferred, and indicates that the routing protocol is considered more 'trustworthy' and is more likely to select good routes.

| Route protocol/type | AD value |
|:-------------------:|:--------:|
| Directly Connected  | 0        |
| Static              | 1        |
| External BGP (eBGP) | 20       |
| EIGRP               | 90       |
| IGRP                | 100      |
| OSPF                | 110      |
| IS-IS               | 115      |
| RIP                 | 120      |
| EIGRP (external)    | 170      |
| Internal BGP (iBGP) | 200      |
| Unusable Route      | 255      |

If the Administrative Distance is 255, the router does not believe the source of that route and does not install the route in the routing table.

### Floating Static Routes

You can change the AD value of any routing protocol and by changing the the AD of a static route, you can make it less preferred than routes learned by a dynamic routing protocol to the same destination (make sure the aD is higher than the routing protocols AD). This is called a **'floating static route'**.

*This **'floating static route'** will be inactive and will not be displayed in the routing table unless the route learned by the dynamic routing protocol is removed.*
