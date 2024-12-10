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

Two addresses cannot be assigned to a host, regardless of the mask, the Network Address and the Broadcast Address. These are denoted by the bits of host portion of the IP address being all 0s or all 1s respectively. Example, using the IP address 192.168.175/24 network. /24 indicates that the first three octets (192.168.1) are the network portion of the IP address and as such, the network can be written as 192.168.1.0/24. This leaves the final octet (175) as the host portion of the IP address. If all the host portion bits are 1s, in this example that would resolve to 255, this is the broadcast address and anything transmitted to this address will transmit to all hosts on the network. If this were to occur, the MAC address FF:FF:FF:FF:FF:FF will be used in the Data Link layer header.

## Part 2

- Maximum number of hosts per network
  - 2<sup>n</sup> - 2
  - Determine the number of bits assigned for hosts (*n*)
    - 32 - *prefix number*
  - Raise 2 to *n*
    - 2<sup>n</sup>
  - Subtract 2 for the network and broadcast address
  - Example: 10.0.0.0/8
    - 32 - 8 = 24
    - 2<sup>24</sup> = 16,777,216
    - 16777216 - 2 = 16,777,214 max number of hosts
  - Example: 192.168.0.0/24
    - 32 - 24 = 8
    - 2<sup>8</sup> = 256
    - 256 - 2 = 254
  - **Tips for estimating the number of host**
    - It is a logarithmic scale, so the number of hosts approximately doubles for each extra bit.
    - 10 and 20 are key values.
      - 10 is approx 1,000 (2<sup>10</sup> = 1024)
      - 20 is approx 1,000,000 (2<sup>20</sup> = 1,048,576)
    - Double for each "1s" value
      - Example: /16
        - 10 approx 1000
        - 11 approx 2000
        - 12 approx 4000, etc
        - 16 approx 64,000 (65,536)
- A Router will normally have a different IP address, to a different network, assigned to each interface.

### Commands

- From Priv EXEC Mode (`#`)
  - Displays a brief description of each interface.
    - `show ip interface brief`
      - `Status` column is the Layer 1 status
        - ie. is the cable plugged in
      - `Protocol` is the Layer 2 protocol status
    - Confirms each interface's status.
  - Show detailed information of an interface
    - `show interfaces <interface ID>`
  - Show the admin set descriptions of all interfaces
    - `show interfaces description`
    - How to set is below.

- From Interface Config Mode (`(config-if)#`)
  - Can enter Interface Config Mode from Global Config Mode
    - `conf t`
    - `int <interface ID>`
      - `int g0/1`
  - Set an IP address for the interface
    - `ip address <ip address> <subnet mask>`
      - `ip address 10.255.255.254 255.0.0.0`
    - `no shutdown`
      - Enables the interface, or brings the interface up
  - Set a description for an interface
    - `description <description>`
