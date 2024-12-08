# IP Addressing

## Part 1

- IP Address Length:
  - 4 bytes
  - 32 bits
  - 8 octets
- IP Addresses are displayed as dotted decimal
  - Transmitted as a binary stream

### IP Address Classes

| Class | First Octet | First Octet Numeric Range | Prefix Length |
|:-----:|:-----------:|:-----------------------------:|:-----------------:|
|   A   |   0xxxxxxx  |            0 - 127            |         /8        |
|   B   |   10xxxxxx  |           128 - 191           |        /16        |
|   C   |   110xxxxx  |           192 - 223           |        /24        |
|   D   |   1110xxxx  |           224 - 239           |         -         |
|   E   |   1111xxxx  |           240 - 255           |         -         |

Of note, the 127.0.0.1 - 127.255.255.254 range is reserved as the loopback address.

- Netmasks and Prefixs
  - The netmask is an alternative way of writing the prefix, using dotted decimal.
    - Class A: /8
      - 255.0.0.0
    - Class B: /16
      - 255.255.0.0
    - Class C: /24
      - 255.255.255.0
  - Whichever method is used, it denotes the number of bits assigned to the network portion of the IP address.
    - The remaining bits are assigned to the host portion
    - Example. 255.255.255.0 or /24
      - This resolves to the first three octets being the network portion of the IP address
      - the final octet being the host portion

Two addresses cannot be assigned to a host, regardless of the mask, the Network Address and the Broadcast Address. These are denoted by the bits of host portion of the IP address being all 0s or all 1s respectively. Example, using the IP address 192.168.175/24 network. /24 indicates that the first three octets (192.168.1) are the network portion of the IP address and as such, the network can be written as 192.168.1.0/24. This leaves the final octect (175) as the host portion of the IP address. If all the host portion bits are 1s, in this example that would resolve to 255, this is the broadcast address and anything transmitted to this address will transmit to all hosts on the network. If this were to occur, the MAC address FF:FF:FF:FF:FF:FF will be used in the Data Link layer header.

## Part 2
