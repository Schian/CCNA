# Layer 2 Discovery Protocols

- Layer 2 Discovery Protocols share information with and discover information about connected, neighbouring devices
  - Hostname, IP address, device type, device capabilities, etc
- Due to all this sharing, they can be considered a security risk and often not used

## Cisco Discovery Protocol

- **Cisco Discovery Protocol (CDP)** is a proprietary Cisco protocol
- Enable by default on Cisco devices
- CDP messages are sent to multicast MAC address `0100.0ccc.cccc`
- Once received, the devices processes then drops the information
  - The messages are not forwarded to other devices
- By default, CDP messages are sent one every **60 seconds**
- By default, the CDP holdtime is **180 seconds**
  - If a message isn't received from a neighbour for the holdtime, the neighbour is removed from the CDP neighbour table
- CDPv2 messages are sent by default

## Link Layer Discovery Protocol

- **Link Layer Discovery Protocol (LLDP)** is an industry standard
  - IEEE 802.1ab
- Usually disabled by default on Cisco devices
  - When enabling, it must be enabled globally and then each interface must enable both Tx and Rx
  - A device can run both CDP and LLDP at the same time
- LLDP messages are sent to multicast MAC address `0180.c200.000e`
- Once received, the devices processes then drops the information
  - The messages are not forwarded to other devices
- By default, LLDP messages are sent once every **30 seconds**
- By default, the LLDP holdtime is **120 seconds**
- LLDP has an additional timer called *'Reinitialization delay'*
  - If enabled, this timer will delay the actual initialisation of LLDP
    - **2 seconds** by default

## Configuration

### Show commands

- Show basic information (timers, version)
  - `R1#show {cdp | lldp}`
- Display how many messages have been sent and received
  - `R1#show {cdp | lldp} traffic`
- Display interface information
  - `R1#show {cdp | lldp} interface`
- List neighbours with basic information
  - `R1#show {cdp | lldp} neighbors`
- List each neighbour with more detailed information
  - `R1#show {cdp | lldp} neighbors detail`
- Display the same information above, but for a specific neighbour only
  - `R1#show {cdp | lldp} entry <device hostname>`

### CDP configuration

- Enable/disable CDP globally
  - `R1(config)#[no] cdp run`
- Enable/disable CDP on an interface
  - `R1(config-if)#[no] cdp enable`
- Configure the CDP timer
  - `R1(config)#cdp timer <seconds>`
- Configure the CDP holdtime
  - `R1(config)#cdp holdtime <seconds>`
- Enable/disable CDPv2
  - `R1(config)#[no] cdp advertise-v2`
    - Disabling v2 will revert to v1

### LLDP Configuration

- Enable LLDP globally
  - `R1(config)#lldp run`
- Enable LLDP Tx on an interface
  - `R1(config-if)#lldp transmit`
- Enable LLDP Rx on an interface
  - `R1(config-if)#lldp receive`
- Configure the LLDP timer
  - `R1(config)#lldp timer <seconds>`
- Configure the LLDP holtime
  - `R1(config)#lldp holdtime <seconds>`
- Configure the LLDP reinit timer
  - `R1(config)#lldp reinit <seconds>`
