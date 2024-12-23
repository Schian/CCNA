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

[Playlist](https://www.youtube.com/playlist?list=PLIFyRwBY_4bQUE4IB5c4VPRyDoLgOdExE)

### Draw the Cheat Sheet

1. Start with **1**, double until you reach **128**. Write from right-to-left.
2. Subtract top row from **256**
3. From /32, list the CIDR notations. Write from right-to-left.

|                   |     |     |     |     |     |     |     |     |
|:-----------------:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Group Size**    | 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
| **Subnet Mask**   | 128 | 192 | 224 | 240 | 248 | 252 | 254 | 255 |
| **CIDR Notation** | /25 | /26 | /27 | /28 | /29 | /30 | /31 | /32 |

- The following seven attributes can be found with the cheatsheet
  - Network ID
  - Broadcast IP
  - First Host IP
  - Last Host IP
  - Next Network
  - Total IP Addresses
  - CIDR/Subnet Mask

1. Use the CIDR/Subnet mask to find the column on the cheat sheet
  a. Note the group size for this CIDR/Subnet mask.
  c. Start at ".0" in the relevant octet.
  d. Increment by the group size until you pass the target IP
2. The number BEFORE the target IP is the Network ID
3. The number AFTER the target IP is the Next Network
4. The IP address BEFORE the Next Network is the Broadcast IP
5. The IP address AFTER the Network ID is the the First Host
6. The IP address BEFORE the Broadcast IP is the Last Host
7. The total number of IP address is the group size (-2 for useable)

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
  a. The provided IP has the prefix /28, this maps to .240 (255.255.255.240)
  b. Note the Group Size for this CIDR/Subnet mask. This is 16
  c. Start at ".0" in the relevant octet. Octet 4 in this case.
  d. Increment by the group size until you pass the target IP (.55)
    1. .0 -> .16 -> .32 -> .48 -> .65
2. The number BEFORE the target IP is the Network ID
  a. Our target is .55, the increment before this is .48
  b. **Network ID Found**
3. The number AFTER the target IP is the Next Network
  a. Our target is .55, the increment after this is .65
  b. **Next Network Found**
4. The IP address BEFORE the Next Network is the Broadcast IP
  a. .64 minus 1 is .63
  b. **Broadcast IP Found**
5. The IP address AFTER the Network ID is the the First Host
  a. .48 plus 1 is .49
  b. **First Host Found**
6. The IP address BEFORE the Broadcast IP is the Last Host
  a. .63 minus 1 is .62
  b. **Last Host Found**
7. The total number of IP address is the group size (-2 for useable)
  a. Group size is 16
  b. **Total IP Addresses Found**
