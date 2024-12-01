# Ethernet LAN Switching

## Part 1

- Ethernet LAN switching occurs at layer 2 (Data-link)
  - Protocol Data Unit (PDU) is a frame
    - consisting of L2 header and a L2 trailer
      - encapsulating L3 header, L4 header, and the data
- A LAN is (normally) bound by the interface on a router
  - While more nodes can be connected to a router, these can be entirely separate networks
    - Subnets and VLANs will come later.
- There can be as many switches as necessary in a LAN
  - Switches operate at layer 2
- Ethernet Frame consists of a header and a trailer
  - 26 bytes in total (header:22 + trailer:4)
  - Fields lengths: 7, 1, 6, 6, 2, 4
  - **Header**
    - Preamble
      - 7 bytes (56 bits) reversals
      - Clock synchronisation
    - Start Frame Delimiter (SFD)
      - 1 byte (8 bits)
        - 0b10101011 or 0xAB
      - Marks end of preamble
    - Destination and Source MAC Address fields
      - 6 bytes (48 bits) each
      - Media Access Control
    - Type (or Length) field
      - 2 bytes (16 bits)
      - 0d1500 or less indicates the LENGTH of the encapsulated packet
      - 0d1536 or greater indicates the TYPE of the encapsulated packet and the length is determined by other means
        - 0d2048 (0x800) is IPv4
        - 0d34525 (0x86DD) is IPv6
  - **Trailer**
    - Frame Check Sequence (FCS)
      - 4 bytes CRC (32 bits)
- MAC Addresses
  - 6 byte address
    - First 3 bytes are the OUI (Organisationally Unique Identifier)
  - Assigned when the device is manufactured
  - Written as 12 hex characters
- MAC Address Table
  - Dynamic Learned MAC Address or simply Dynamic MAC address
    - As the switch receives frames, it stores the source MAC Address and the interface it came in on
    - By default on Cisco devices, Dynamic addresses are removed after 5 minutes
  - Unknown Unicast Frame: FLOOD
    - If the destination MAC address is not in the MAC Address table, the switch will flood all interfaces (except the one the frame came in on).
  - Known Unicast Frame: FORWARD
    - If the destination MAC Address is in the MAC Address table, the switch will only forward the frame to the mapped interface.
  