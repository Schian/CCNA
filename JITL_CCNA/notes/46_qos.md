# Quality of Service

## IP Phones, Voice VLANs and Power over Ethernet

- IP Phones use VoIP (Voice over IP) to enable calles over an IP network
  - IP phones are connected to a switch just like any other end host
- IP phones have an internal 3-port switch
  - 1 port is the 'uplink' to the external switch
  - 1 port is the 'downlink' to the PC
  - 1 port connects internally to the phone itself
  - This allows the PC and the IP phone to share a single switch port
- It is recommended to separate 'voice' traffic (from the IP phone) and 'data' traffic (form the PC) by placing them in separate VLANs
  - This can be accomplished using a *voice VLAN*
    - Traffic from the PC will be untagged
    - Traffic from the phone will be tagged with a VLAN ID
  - Although the interface sends/receives traffic from two VLANs, it is not considered a trunk port
	- It is considered an access port
	- `show interface <intID> trunk` will show the "Status" as "not-trunking"

<br>

- Power over Ethernet (POE) allows Power Sourcing Equipment (PSE) to provide power to Powered Devices (PD) over an ethernet cable
  - Typically the PSE is a switch and the PDs are IP phones, IP cameras, wireless access points, etc
  - The PSE receives AC power from the outlet, converts it to DC power and supplies that power to the PDs
- Too much electrical current can damage electrical devices
  - PoE has a process to determine how much power a connected device needs
    - When a device is conected to a PoE-enabled port, the PSE (switch) sends low power signals and monitors the response to determine how much power the PD needs, if at all
	- If the device needs power, the PSE supplies the power to allowed the PD to boot
	- The PSE continues to monitor the PD and supply the required amount of power
- *Power policing* can be configured to prevent a PD from taking too much power
  - **Insert a link here to the config**
  - `power inline police` configures power policing with the default settings
    - Disable the port and send a Syslog message if a PD draws too much power
	- Equivalent to `power inline police action errdisable`
	- The interface will be put into an 'error-disabled' state
	  - Can be re-enabled with `shutdown` followed by `no shutdown`
  - `power inline police action log` does not shut down the interface if the PD draws too much power
    - It will restart the interface and send a Syslog message

## Quality of Service

### Part 1

- Modern networks are typically *converged networks* in which IP phones, video traffic, regular data traffic, etc all share the same IP network
- This enables cost savings as well as more advanced features for voice and video traffic
  - Integration with collaboration software
    - Cisco WebEx
	- Microsoft Teams
	- Etc
- However, the different kinds of traffic now have to compete for bandwidth
- Quality of Service (QoS) is a set of tools used by network devices to apply different treatment to different packets
- QoS is used to manage the following characteristics of network traffic:
  - **Bandwidth**
	- The overall capacity of the link, measured in bits per second
	- QoS tools allow you to reserve a certain amount of a link's bandwidth for specific kinds of traffic
  - **Delay**
    - One-way delay: The amount of time it taks traffic to go from source to destination
	- Two-way delay: The amount of time it taks traffic to go from source to destination and return
  - **Jitter**
    - The variation in one-way delay between packets sent by the same application
	- IP phones have a 'jitter buffer' to provice a fixed delay to audio packets
  - **Loss**
    - The % of packets sent that do not reach their destination
	- Can be caused by faulty cables
	- Can also be caused when a device's packet *queues* get full and the device starts discarding packets.
- Recommendations for acceptable ineractive audio quality
  - One-way delay: 150ms or less
  - Jitter: 30ms or less
  - Loss: 1% or less

#### Queuing

### Part 2

## Configuration

### Voice VLANs and PoE - Configuration

- Enable a voice VLAN on the switchport
  - `SW1(config)#interface g0/0`
  - `SW1(config-if)#switchport more access`
  - `SW1(config-if)#switchport voice vlan <number>`
  - The connected PC will send traffic untagged as normal.
  - SW1 will use CDP to tell the phone to its traffic as `VLAN <number>`
    - A second VLAN can be created for the PC 'data' traffic
	- `SW1(config-if)#access vlan <number>`
- Show the "power inline police" information
  - `SW1#show power inline police <interface ID>`
- Configure default PoE settings on an interface
  - `SW1(config-if)#power inline police`
- Configure the interface to send a syslog message and restart instead of errdisable
  - `SW1(config-if)#power inline police action log`

### QOS - Configuration
