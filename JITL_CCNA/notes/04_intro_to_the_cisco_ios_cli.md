# Day 4 - Introduction to the Cisco IOS CLI

## Establish connection

- Command Line Interface used to configure Cisco devices
- Connect to a device through the console port
  - Bring laptop to device and connect
  - Must be done for the first, out-of-box setup
  - One can be an RJ45 port. Called a rollover cable.
    - RJ45-to-DB9 (serial), will likely need a USB adaptor
    - Works like a crossover cable
      - pin 1 to pin 8
      - 2 to 7
      - 3 to 6
      - so on
- Use a terminal emulator like `PuTTY` or `Minicom`
  - Ensure the settings are as follows:
    - 9600 Bd
    - 8N1
      - 8 data bits
      - no parity
      - 1 stop bit
    - No flow control

## Now connected

- By default, the first time entering the CLI I am in `User EXEC mode`
  - This is indicated by the `>` '**greater-than**' symbol next to the hostname
  - Also known as `User mode`
  - `User EXEC mode` is limited. Can read some things, no write privileges.
- Change to `Privileged EXEC mode` by typing `enable` or `en` + `TAB`
  - Indicated by the `#` next to the hostname
  - Complete read access and can restart the device
  - Cannot change the configuration
    - Can change time
    - Can save config file
- Can use `?` to view all commands available
- Change to `Global Configuration Mode` by typing `configure terminal` or `conf t` + `TAB`
  - `Global Configuration Mode` is indicated by `(config)` in the prompt
- `do <privileged-exec-level command>` will execute a privileged-exec level command from `Global Configuration Mode`

## Configuration Files

- **Running-config**: The current, active configuration file on the device. As you enter commands on the CLI, you edit the active configuration.
  - `show running-config`
- **Startup-config**: The configuration file that will be loaded upon restart of the device.
  - `show startup-config`
- To write the **Running-config** to the **Startup-config** there are three methods to achieve this.
  - All three methods must be run from `Privileged EXEC mode`
  - `write`
  - `write memory`
  - `copy running-config startup-config`

- **Encrypt passwords**
  - *Method 1*
    - Enter `Global Configuration Mode` with `conf t`
    - `service password-encryption`
      - Cisco's proprietary encryption
      - This isn't very secure
  - *Method 2*
    - Enter `Global Configuration Mode` with `conf t`
    - `enable secret <Password>`
      - This will use hash the password with an MD5 hash.
- If `service password-encryption` is **enabled**
  - Current passwords will be encrypted
  - Future password will be encrypted
  - `enable secret` will not be effected
- If `service password-encryption` is **disabled**
  - Current passwords will not be decrypted
  - Future passwords will not be encrypted
  - `enable secret` will not be effected

## Canceling Commands

- Simply putting `no` in front of a command will cancel or disable that command
  - `no service password-encryption`
