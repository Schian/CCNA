# Network Address Translation

## Part 1

- **RFP 1918** specifies the following IPv4 address ranges as private:
  - **`10.0.0.0/8`** (`10.0.0.0` to `10.255.255.255`)
  - **`172.16.0.0/12`** (`172.16.0.0` to `172.31.255.255`)
  - **`192.168.0.0/16`** (`192.168.0.0` to `192.168.255.255`)

### NAT

- Network Address Translation (NAT) is used to modify the source and/or destination address of packets
- The most common use for NAT is to allow hosts with private IP address to communicate with other hosts over the internet
- The focus will be on **source NAT** for the CCNA
  - The router will translate (change) the source IP of outgoing packets and then translate (change) the destination of incoming packets
  - How the addresses are translated depends on the type of NAT being used

#### Static NAT

- **Static NAT** involves statically configuring one-to-one mappings of private IP address to public IP address
  - Since this requires a one-to-one it doesn't help to preserve IP addresses, but is a good learning tool
- An *Inside Local* IP address is mapped to an *Inside Global* IP address
  - **Inside Local**: The IP address of the *inside* host, from the perspective of the local network
    - The IP address actually configured on the inside host
  - **Inside Global**: The IP address of the *inside* host, from the perspective of the outside hosts
    - After NAT, how the outside hosts will see the IP of the host
  - **Outside Local**: The IP address of the *outside* host, from the perspective of the local network
  - **Outside Global**: The IP address of the *outside* host, from the perspective of the outside network
    - Both Outside Local/Global should be the same in `show ip nat translations`
    - They will only be different if **destination NAT** is being used
- **Inside/Outside**: Location of the host
- **Local/Global**: Perspective

## Part 2

## Part 1 - Configuration

- Show current NAT translations
  - `R1#show ip nat translations`
- Remove all dynamic nat translations
  - `R1#clear ip nat translation *`
- Define the inside interface
  - `R1(config)#int g0/1`
  - `R1(config-if)#ip nat inside`
- Define the outside interface
  - `R1(config)#int g0/0`
  - `R1(config-if)#ip nat outside`
- Configure the one-to-one IP address mapping
  - `R1(config)#ip nat inside source static <inside local ip> <inside global ip>`
- Show NAT statistics
  - `R1#show ip nat statistics`

## Part 2 - Configuration
