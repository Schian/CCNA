# Subnetting

## Part 1

- Classless Inter-Domain Routing (CIDR)
  - Allows larger A, B, C class IPv4 networks to be broken down in to smaller subnetworks
    - Reducing the number of unused useable addresses in the network
- Increases the number of wasted address (more broadcast and network addresses)
  - Bits from the host portion of the network address are reallocated to the network portion
    - As the prefix length increases:
  - Number of networks increases
  - Number of hosts per network decreases
- Inverted when the length decreases.
- the /31 network prefix is used for a point-to-point network
- the /32 prefix is used to specify an exact host or interface

| **Dotted Decimal** | **CIDR Notation** |
|:------------------:|:-----------------:|
| 255.255.255.128    | /25               |
| 255.255.255.192    | /26               |
| 255.255.255.224    | /27               |
| 255.255.255.240    | /28               |
| 255.255.255.248    | /29               |
| 255.255.255.252    | /30               |
| 255.255.255.254    | /31               |
| 255.255.255.255    | /32               |

## Part 2

- Number of subnets = 2^x
  - Where x is the number of "borrowed bits"
  - If 3 bits from the host portion is "borrowed" by the network portion, 8 more networks can be created.
- To identify what subnet an IP address belongs to, calculate the value of the "borrowed" bits. (Set remaining host bits to 0)
  - 192.168.29.219/29
  - 219 = 11011011
    - the first 11011 are the "borrowed" bits
  - 11011000 = 216
  - IP belongs to the .216 network
- Subnetting Class B Networks
  - The process is almost the same, except bits are borrowed from the third octet
  - Example:
    - Create 80 subnets from 172.16.0.0/16
  - 2^x > 80, 2^7 = 128
  - The value of 7 borrowed bits is 254
  - 172.16.0.0/23
    - 172.16.0.0/23 - 172.16.2.0/23 - 172.16.4.0/23 etc
- What subnet does 172.25.217.192/21 belong to?
  - There are 5 "borrowed" bits
    - 21 - 16 = 5 (prefix length minus Class B prefix length)
  - 217 = 11011001
    - the 11011 are the "borrowed" bits
  - 11011000 = 216
  - IP belongs to the .216.0 network

## Part 3

- Subnetting Class A Networks
  - The process is almost the same, except bits are borrowed from the second octet
  Example:
    - Create 2000 subnets with the 10.0.0.0/8 network
  - 2^x > 2000, 2^11 = 2048
  - 10.0.0.0/19
  - 8190 hosts per subnet
    - 2^13 - 2 = 8190
  - 13 bits remaining for host portion
- Variable Length Subnet Masks (VLSM)
  - The process of creating subnets of different sizes making network use more efficient
  - Steps for VLSM
    - Assign the largest subnet at the start of the address space
- Assign the next largest subnet after it
- Repeat until all subnets have been assigned

## Additional Resources

- [IPv4 Subnetting Practice](https://subnetipv4.com/)
- [Subnetting Questions](https://subnetting.org/)
- [Subnetting Practice](https://subnettingpractice.com/)

## Subnetting Mastery Video Series

[Subnetting Mastery Playlist](https://www.youtube.com/playlist?list=PLIFyRwBY_4bQUE4IB5c4VPRyDoLgOdExE)

### Draw the Cheat Sheet

1. Start with **1**, double until you reach **128**. Write from right-to-left.
2. Subtract top row from **256**
3. From /32, list the CIDR notations. Write from right-to-left.

|                 |     |     |     |     |     |     |     |     |
|:---------------:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Group Size**  | 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
| **Subnet Mask** | 128 | 192 | 224 | 240 | 248 | 252 | 254 | 255 |
| **4th Octet**   | /25 | /26 | /27 | /28 | /29 | /30 | /31 | /32 |
| **3rd Octet**   | /17 | /18 | /19 | /20 | /21 | /22 | /23 | /24 |
| **2nd Octet**   | /9  | /10 | /11 | /12 | /13 | /14 | /15 | /16 |
| **1st Octet**   | /1  | /2  | /3  | /4  | /5  | /6  | /7  | /8  |

- The following seven attributes can be found with the cheatsheet
  - Network ID
  - Broadcast IP
  - First Host IP
  - Last Host IP
  - Next Network
  - Total IP Addresses
  - CIDR/Subnet Mask

1. Use the CIDR/Subnet mask to find the column on the cheat sheet
    1. Note the group size for this CIDR/Subnet mask.
    2. Start at ".0" in the relevant octet.
    3. Increment by the group size until you pass the target IP
2. The number BEFORE the target IP is the Network ID
3. The number AFTER the target IP is the Next Network
4. The IP address BEFORE the Next Network is the Broadcast IP
5. The IP address AFTER the Network ID is the the First Host
6. The IP address BEFORE the Broadcast IP is the Last Host
7. The total number of IP address is the group size (-2 for useable)
   1. For octets 3, 2 and 1 use 2^(32 - CIDR)

---

- Find the following seven attributes for the IP address 10.1.1.55/28
  - Network ID: 10.1.1.48
  - Broadcast IP: 10.1.1.63
  - First Host IP: 10.1.1.49
  - Last Host IP: 10.1.1.62
  - Next Network: 10.1.1.65
  - Total IP Addresses: 16 (14 useable)
  - CIDR/Subnet Mask: /28 and 255.255.255.240

1. Use the CIDR/Subnet mask to find the column on the cheat sheet
    - The provided IP has the prefix /28, this maps to .240 (255.255.255.240)
    - Note the Group Size for this CIDR/Subnet mask. This is 16
    - Start at ".0" in the relevant octet. Octet 4 in this case.
    - Increment by the group size until you pass the target IP (.55)
      - .0 -> .16 -> .32 -> .48 -> .65
2. The number BEFORE the target IP is the Network ID
    - Our target is .55, the increment before this is .48
    - **Network ID Found**
3. The number AFTER the target IP is the Next Network
    - Our target is .55, the increment after this is .65
    - **Next Network Found**
4. The IP address BEFORE the Next Network is the Broadcast IP
    - .64 minus 1 is .63
    - **Broadcast IP Found**
5. The IP address AFTER the Network ID is the the First Host
    - .48 plus 1 is .49
    - **First Host Found**
6. The IP address BEFORE the Broadcast IP is the Last Host
    - .63 minus 1 is .62
    - **Last Host Found**
7. The total number of IP address is the group size (-2 for useable)
    - Group size is 16
    - **Total IP Addresses Found**

---

### Tips for speed

- **Multiply Group Size by 10**
  - 10.3.3.85/29
    - Instead of .0 -> .8 -> .16 -> ... -> .86
    - Do: Group size x 10 = .80 -> .86
- **Double or triple after multiplying by 10**
  - 10.3.3.170/29
    - Group size x 10 = .80 x 2 = .160 -> .168 -> .176
- **All group sizes are a multiple of 128**
  - Can start incrementing from .128 instead of .0 for every group size.
- **Every group size is a multiple of every subnet value to the left**
  - 10.3.3.197/30 has a group size of 4
    - The other tips take more steps than necessary
    - Start incrementing from .192 (the /26 subnet)
      - .192 -> .196 -> .200
- **Subtract instead of add**
  - Use any of the above the speed tips to get close and subtract
    - 10.3.3.117/29
    - .128 -> .120 -> .112

## Fixed Length Subnet Mask (FLSM)

- FLSM Questions are usually made up of three parts
  - Starting Point
  - Ending Point
  - Condition
- The three parts are usually in the form of one of the following:
  - Starting network size
  - Number of sub-networks
  - Size of sub-network

- If you start with a /18, what size sub-network would you need to create 100 sub-networks?
  - Starting Point: /18
  - Ending Point: What size sub-network?
  - Condition: Create 100 sub-networks

We need to determine 2^*n* >= 100 and then add *n* to /18. The quickest way would be to draw out a table and find the correct value (or use the cheatsheet, remember 2^0 is on the right and 2^7 is on the left). In this case *n* = 7 therefore /18 + 7 = /25 is the size of each sub-network.

- If you start with a /20, how many subnetworks could you create that could contain 50 IP addresses?
  - Starting Point: /20
  - Ending Point: How many subnetworks?
  - Condition: Each subnetwork must contain 50 IP addresses

We can quickly determine the the size of the sub-networks will be /26. Therefore we can do /26 - /20 = 6 for the remaining number of host bits, 2^6 = 64 which is the correct answer.
