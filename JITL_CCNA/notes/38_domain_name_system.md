# Domain Name System

## Purpose

- DNS is used to resolve human-readable names to IP addresses
  - Machines don't use names, they use IP addresses
- DNS servers can be manually configured or leaned via DHCP
- DNS records to note:
  - A record
    - Resolves a domain name to an IPv4 address
  - AAAA record
    - Resolves a domain name to an IPv6 address
  - CNAME
    - Canonical Name, used as an alias for an A or AAAA record

## DNS in Cisco IOS

- DNS as a service doesn't have to be configured to be used
  - The router can simply forward DNS messages like other packets
  - Assuming the end host has a DNS server set or leaned via DHCP
- A router can be configured as a DNS server, although it is rare
  - Usually an internal network will use a Windows or Linux server
- A Cisco router can also be configured as a DNS client
- A Cisco device can be configured to have a default domain name
  - Details will be covered in a later lesson

## Configuration

- List the known hosts and DNS cache
  - `R1#show hosts`
- Configure the router to be a DNS server
  - `R1(config)#ip dns server`
- Configure hostname -> IP mappings
  - `R1(config)#ip host <name> <ip address>`
- Configure a DNS server for the router to use if the record isn't in its host table
  - `R1(config)#ip name-server <ip address>`
- Enable the router to perform DNS queries (enabled by default)
  - `R1(config)#ip domain lookup`
- Configure the default domain name
  - `R1(config)#ip domain name <domainName.com>`
