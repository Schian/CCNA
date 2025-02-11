# TCP and UDP

## Layer 4 Basics

- Provides transparent transfer of data between end hosts.
- Provides (or doesn't provide) various services to applications
  - Reliable data transfer
  - Error recovery
  - Data sequencing
  - Flow control
- Provides Layer 4 addressing
  - **Port Numbers**
    - Identify the Application Layer protocol
    - Provides session multiplexing
  - Ranges designated by IANA (Internet Assigned Numbers Authority)
    - **Well-known** port numbers: 0 - 1023
    - **Registered** port numbers: 1024 - 49151
    - **Ephemeral** port numbers: 49152 - 65535

## TCP

### Transmission Control Protocol

- Connection-oriented
  - Before actually sending data to the destination host, the two hosts communicate to establish a connection
  - Once the connection is established, the data exchanges begins
- Reliable communication
  - The destination host must acknowledge that it received each TCP segment
  - If a segment isn't acknowledged, it is sent again
- Sequencing
  - Sequence numbers in the TCP header allow destination hosts to put TCP segments in the correct order, even if they arrive out of order
- Flow control
  - The destination host can tell the source host to increase/decrease the rate that data is sent

#### TCP Header

The TCP header is 160 bits (20 bytes) in length (generally), notable fields for the CCNA are as follows:

- Source and Destination port fields
  - Each are 16 bits in length
    - 65 536 available port numbers (2<sup>16</sup>)
- Sequence and Acknowledge number fields
  - Each are 32 bits in length
  - Used to provide sequencing for reliable communication
- Flag fields
  - Each flag is one bit in length
  - Used to establish and terminate connections
  - Fields of note:
    - FIN (bit 0)
    - SYN (bit 1)
    - ACK (bit 4)
- Window Size
  - 16 bits in length
  - Used for flow control

#### Establishing Connections: Three-Way Handshake

```txt
--> SYN
<-- SYN-ACK
--> SYN
```

- The client sends a TCP packet with the SYN flag set
- The server responds with a TCP packet with the SYN and ACK flags set
- The client sends a TCP packet with the ACK flag set
- The connection has now been established and data can begin to be transferred

#### Terminating Connections: Four-Way Handshake

```txt
--> FIN
<-- ACK
<-- FIN
--> ACK
```

- Whichever end decides to terminate the connection starts by sending a FIN flag
- The other end responds with an ACK flag, then
- Sends a second TCP packet with a FIN flag
- The first side sends an ACK flag

#### Sequencing and Acknowledgement

- Hosts set a random initial sequence number
- **Forward acknowledgement** is used to indicate the sequence number of the next segment the hosts expects to receive
  - A host will acknowledge the receipt of a packet by acknowledging the next expected sequence number
  - To signal for a retransmission, a host continues to ACK with the next expected sequence number

```txt
--> SYN     Seq: 10
<-- SYN-ACK Seq: 50, Ack 11
--> ACK     Seq: 11, Ack 51
<-- Data    Seq: 51, Ack 12
--> Data    Seq: 12, Ack 52
... etc
```

#### Flow Control: Window Size

- Acknowledging every single segment, is inefficient
- The **Window Size** field allows more data to be sent before an acknowledgement is required
  - A 'sliding window' can be used to dynamically adjust how large the windows size is.

```txt
--> Seq: 20
--> Seq: 21
--> Seq: 22
<-- Ack: 23
```

## UDP

### User Datagram Protocol

- **Not** connection-oriented
  - The sending host does not establish a connection with the destination host before sending data
  - The data is simply sent
- **Does not** provide reliable communication
  - Acknowledgements are not sent for received segments
  - If a segment is lost, there is no mechanism for retransmission
  - Segments are sent *"best effort"*
- **Does not** provide sequencing
  - There is no mechanism to place the segment back in order if they arrive out of order
- **Does not** provide flow control
  - No flow control mechanism

#### UDP Header

The UDP header is 8 bytes (64 bits) in length and only has four fields:

- Source and Destination port
  - Each field is 16 bits in length
    - 65 536 available port numbers (2<sup>16</sup>)
- Length
  - The number of bytes encapsulated in the datagram
- Checksum
  - Provides error checking for the entire UDP datagram

## Comparing the Two

- TCP provides more features than UDP, but at the cost of additional **overhead**
- For applications that required reliable communications, TCP is preferred
- For application like real-time voice and video, UDP is preferred
- There are some applications that use UDP, but provide reliability within the application itself
- Some applications use both TCP & UDP, depending on the situation

| **TCP**                                  | **UDP**                            |
|:----------------------------------------:|:----------------------------------:|
| Connection-oriented                      | Connectionless                     |
| Reliable                                 | Unreliable                         |
| Sequencing                               | No Sequencing                      |
| Flow Control                             | No Flow Control                    |
| Used for downloads,<br>file sharing, etc | Used for real-time<br>applications |

## Notable Ports

| **TCP**          | **UDP**            | **TCP & UDP** |
|------------------|--------------------|---------------|
| FTP data (20)    | DHCP server (67)   | DNS (53)      |
| FTP control (21) | DHCP client (68)   |               |
| SSH (22)         | TFTP (69)          |               |
| Telnet (23)      | SNMP agent (161)   |               |
| SMTP (25)        | SNMP manager (162) |               |
| HTTP (80)        | Syslog (514)       |               |
| POP3 (110)       |                    |               |
| HTTPS (443)      |                    |               |
