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

### Dynamic NAT

- In **dynamic NAT**, the router dynamically maps *inside local* address to *inside global* addresses as needed
- An ACL is used to identify which traffic should be translated
  - If the source IP is **permitted** by the ACL, the source IP will be translated
  - If the source IP is **denied** by the ACL, the source IP will **NOT** be translated
    - The traffic won't be dropped, it will still be routed
- A NAT pool is used to define the available *inside global* addresses
- If there aren't enough *inside global* IP addresses available, it is called 'NAT pool exhaustion'
  - If a packet from another inside host arrives and needs NAT but there are no available addresses, the router will drop the packet
  - Dynamic NAT entries will timeout automatically if not used
    - They can also be cleared manually

### PAT (Nat Overload)

- Port Address Translation (PAT) translates both the IP address and the port number
  - It will use a unique port number for each communication flow
  - A single public IP address can be used by many different internal hosts
    - Port number field is 16 bits == over 65 000 available ports
- Because many inside hosts can share a single public IP, PAT is very useful for preserving public IP addresses

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

### Dynamic NAT - Configuration

```cisco
# Define the inside and outside nat interfaces
R1(config)#int g0/1
R1(config-if)#ip nat inside
R1(config-if)#int g0/0
R1(config-if)#ip nat outside

# Create the ACL and define the traffic to be translated
R1(config-if)#exit
R1(config)#access-list <number> permit <ip address> <wildcard mask>

# Define the pool of inside global IP addresses
R1(config)#ip nat pool <pool-name> <start ip> <end ip> prefix-length <length>

# Configure dynamic NAT by mapping the ACL to the pool
R1(config)#ip nat inside source list <acl> pool <pool-name>
```

### PAT - Configuration

#### Using a Pool

```cisco
# Almost the same as dynamic NAT

# Define the inside and outside nat interfaces
R1(config)#int g0/1
R1(config-if)#ip nat inside
R1(config-if)#int g0/0
R1(config-if)#ip nat outside

# Create the ACL and define the traffic to be translated
R1(config-if)#exit
R1(config)#access-list <number> permit <ip address> <wildcard mask>

# Define the pool of inside global IP addresses
# This pool can be much smaller than Dynamic NAT
R1(config)#ip nat pool <pool-name> <start ip> <end ip> prefix-length <length>

# Configure dynamic NAT by mapping the ACL to the pool
# Same command EXCEPT for `overload` at the end
R1(config)#ip nat inside source list <acl> pool <pool-name> overload
```

#### Using the Router's Outside Interface

```cisco
# The same at the start

# Define the inside and outside nat interfaces
R1(config)#int g0/1
R1(config-if)#ip nat inside
R1(config-if)#int g0/0
R1(config-if)#ip nat outside

# Create the ACL and define the traffic to be translated
R1(config-if)#exit
R1(config)#access-list <number> permit <ip address> <wildcard mask>

# Configure PAT by mapping the ACL to the interface and enabling overload
R1(config)#ip nat inside source list <acl> interface <ID> overload
```
