# Port Security

## Lecture

- Port security is a security feature of Cisco switches
  - Allowing you to control which source MAC addresses are allowed to enter the switchport
  - If an unauthorised source MAC address enters the port, action will be taken
    - The default action is to place the interface in an `err-disabled` state
- The maximum number of MAC addresses can be configured
  - The MAC address can be manually configured, dynamically learned, or both
- Port security allows network admins to control which devices are allowed to access the network
- However, MAC address spoofing is a simple task
  - It's easy to configure a devices to send frames with a different source MAC address
- Rather than manually specifying the MAC addresses allowed on each port, port security can limit the number of MAC addresses allowed on an interface
  - This can be more useful, and can help prevent DHCP starvation attacks

### Enabling Port Security

- Port security can only be enabled on statically configured access ports or trunk ports
  - `switchport mode access` or `switchport more trunk`
  - `switchport mode dynamic {auto | desirable}` will return an error message
    - `Command rejected: <interface> is a dynamic port.`

### Violation Modes

- There are three different violation modes that determine what the switch will do if an unauthorised frame enters an interface configured with port security
- **Shutdown**
  - Effectively shuts down the port by placing it in an err-disabled state
  - Generates a Syslog and/or SNMP message when the interface is disabled
  - The violation counter is set to $1$ when the interface is disabled
- **Restrict**
  - The switch discards traffic from unauthorised MAC addresses
  - The interface is **NOT** disabled
  - Generates a Syslog and/or SNMP message each time an unauthorised MAC is detected
  - The violation counter is incremented by $1$ for each unauthorised frame
- **Protect**
  - The switch discarded traffic from unauthorised MAC addresses
  - The interface is **NOT** disabled
  - It does **NOT** generate a Syslog or SNMP messages for unauthorised traffic
  - It does **NOT** increment the violation counter

### Secure MAC address aging

- By default, secure MAC address will not 'age out'
  - `Aging Time: 0 mins`
  - Can be configured with `switchport port-security aging time <min>`
- The are two aging settings
  - **Absolute**
    - After the secure MAC address is learned, the aging time starts and the MAC is removed after the timer expires
    - Even if the switch continues receiving frames from that source MAC address
  - **Inactivity**
    - After the secure MAC address is learned, the aging time starts but is reset after every frame received from that source MAC address on the interface.

### Sticky Secure MAC Addresses

- 'Sticky' secure MAC address learning can be enabled
  - `switchport port-security mac-address sticky`
- When enabled, dynamically-learned secure MAC addresses will be added to the running config
  - `switchport port-security mac-address sticky <MAC>`
  - Must be written to the startup config to be 'remembered'
    - `write memory`
- When the command is issued, all current dynamically-learned secure MAC address will be converted to sticky secure MAC addresses
- If you issue the `no switchport port-security mac-address sticky` command, all current sticky secure MAC addresses will be converted to regular dynamically-learned secure MAC addresses.
- Configuring sticky secure MAC addresses is the **ONLY WAY** for the switch to remember a MAC address if a device is unplugged

## Configuration

- Show switchport configuration settings
  - `SW1#show interface <int ID> switchport`
- Show port-security configuration
  - `SW1#show port-security interface <int ID>`
- Show the secure MAC addresses learned or configured
  - `SW1#show mac address-table secure`
- Enable port security with default settings
  - `SW1(config)#interface g0/1`
  - `SW1(config-if)#switchport mode access`
  - `SW1(config-if)#switchport port-security`
- Enable port security, setting the violation mode
  - `SW1(config-if)#switchport port-security violation {shutdown | restrict | protect}`
- Manually configure a static MAC address to be allowed
  - `SW1(config-if)#switchport port-security mac-address <MAC>`
- Configure the maximum number of addresses allowed on the interface
  - `SW1(config-if)#switchport port-security maximum <number>`
- Re-enable an interface
  - Manually
    - `SW1(config-if)#shutdown`
    - `SW1(config-if)#no shutdown`
  - ErrDisable Recovery
    - `SW1(config)#errdisable recovery cause psecure-violation`
    - `SW1(config)#errdisable recovery interval <seconds>`
- Configure MAC address aging
  - Configure the aging time
    - `SW1(config-if)#switchport port-security aging time <minutes>`
  - Configure the aging type
    - `SW1(config-if)#switchport port-security aging type {absolute | inactivity}`
  - Configure static MAC aging
    - `SW1(config-if)#switchport port-security aging static`
- Enable sticky secure MAC address learning
  - `SW1(config-if)#switchport port-security mac-address sticky`
    - Disable with `no`
