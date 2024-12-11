# IPv4 Header

- The IPv4 header consists of 14 fields of which 13 are required.
- There is no error detection on the data, this is handled by both layer 4 and layer 2.
- The header can be between 20 - 60 bytes.
  - 20 bytes for no encapsulated data
  - 60 bytes if the maximum amount of options are used.

## Header Structure

1. **Version**. 4 bits.
   1. 0100 (4) for IPv4
   2. 0110 (6) for IPv6
2. **IHL**. 4 bits.
   1. Internet Header Length
   2. Specifies the number of bytes in the header in multiples of 4
   3. Min value is 0101 (5) for a 20 byte header
   4. Max value is 1111 (15) for a 60 byte header
3. **DSCP**. 6 bits.
   1. Differentiated Services Code Point
   2. Used to prioritise delay-sensitive data
   3. Used for QoS (Quality of Service)
4. **ECN**. 2 bits.
   1. Explicit Congestion Notification
   2. Provides end-to-end notification of network congestion **without dropping packets**
5. **Total Length**. 16 bits.
   1. The total length of the packet (L3 header + L4 header + data)
   2. Min value is 20. No encapsulated data
   3. Max value is 65,535.
6. **Identification Field**. 16 bits.
   1. If a packet is fragmented, used to identify which packet the fragments belong to.
7. **Flags**. 3 bits.
   1. Control and identify fragments
   2. Bit 0, reserved and always set to 0
   3. Bit 1, DF (Don't Fragment)
   4. Bit 2, MF (More Fragments)
      1. Set to 1 if more fragments
      2. Set to 0 if last fragment (or no fragments)
8. **Fragment Offset**. 13 bits.
   1. Indicates the position of the fragment
9. **Time to Live**. 8 bits.
   1. Prevents infinite loops, with routers dropping packets with TTL of 0
   2. Recommended default is 64
10. **Protocol**. 8 bits.
    1. The encapsulated layer 4 protocol
    2. Key ones to remember:
       1. 1 = ICMP
       2. 6 = TCP
       3. 17 = UDP
       4. 89 = OSPF
11. **Header Checksum**. 16 bits.
    1. Error detection for the header.
    2. Router drops packet if the calculated checksum fails
12. **Source Address**. 32 bits.
    1. IPv4 Address of the sender
13. **Destination Address**. 32 bits.
    1. IPv4 Address of the receiver
14. **Options**. 0 - 320 bits (40 bytes)
    1. Rarely used
    2. If IHL field > 5
