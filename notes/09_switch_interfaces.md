# Switch Interfaces

## Notes

- All interfaces are `up/up` by default
  - `down/down` is displayed if nothing is plugged in
  - It is advised to set unused interfaces to `administratively down` for security reasons
- **Hubs**
  - A hub is set to half duplex and every device connected is in the same *collision domain*
    - A collision is when a device in half duplex receives two or more frames at the same time and then tries to transmit those frames.
    - A collision domain is a network segment where devices share the transmission medium and collisions can occur.
      - A switch will all interfaces set to full duplex will have every single interface in its own collision domain
- **Carrier Sense Multiple Access with Collision Detection** (CSMA/CD)
  - Before sending frames, devices ‘listen’ to the collision domain until they detect that other devices are not sending.
  - If a collision does occur, the device sends a jamming signal to inform the other devices that a collision happened.
  - Each device will wait a random period of time before sending frames again.
  - The process repeats.
- **Errors**
  - Errors can be seen at the bottom of the output from `show interfaces <int ID>`
  - **Runts**: Frames smaller than the minimum frame size (64 bytes)
  - **Giants**: Frames larger than the max frame size (1518 bytes)
  - **CRC**: Frames that fail the CRC check
  - **Frame**: Frames with an incorrect format (do to an error)
  - **Input Errors**: Total of all incoming errors
  - **Output Errors**: Total of all outgoing errors

### Commands

- From Priv EXEC Mode (`#`)
  - Displays a brief description of each interface.
    - `show ip interface brief`
  - Display the status of interfaces
    - `show interfaces status`
- From global config mode (`(config)`)
  - Select a range of interfaces to configure
    - `interface range f0/2 - 24`
    - `interface range f0/5 - 15, g0/1 - 2`
- From the interface configuration mode (`(config-if)#`)
  - Set the interface speed
    - `speed <value or auto>`
  - Set the duplex
    - `duplex <auto, full, half>`
    - If both devices don't have the same duplex the status might show as not connected until the settings align.
