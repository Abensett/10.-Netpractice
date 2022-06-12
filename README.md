# 10.-Netpractice

## Table of Contents
1. [Internet Protocol (IP)](#internet-protocol-ip)
2. [Netmask](#netmask)
3. [Transmission Control Protocol (TCP)](#transmission-control-protocol-tcp)
4. [User Datagram Protocol (UDP)](#user-datagram-protocol-udp)
5. [Open Systems Interconnection (OSI) Model](#open-systems-interconnection-osi-model)
6. [Dynamic Host Configuration Protocol (DHCP)](#dynamic-host-configuration-protocol-dhcp)
7. [Domain Name System (DNS)](#domain-name-system-dns)
8. [Ping](#ping)

## Internet Protocol (IP)
- IPv4 addresses are 32 bits long *(4,294,967,296 or 2^32 addresses)*
- IPv6 addresses are 128 bits long *(3.4E38 addresses)*

### IPv4 Address Classes
| *Class* | *Address Range*                 | *Number of Networks* | *Addresses per Network* | *Number of Addresses*   |
| :-----: | :-----------------------------: | :------------------: | :---------------------: | :---------------------: |
| Class A | `0.0.0.0` - `127.255.255.255`   | 128 *(2^7)*          | 16,777,216 *(2^24)*     | 2,147,483,648 *(2^31)*  |
| Class B | `128.0.0.0` - `191.255.255.255` | 16,384 *(2^14)*      | 65,536 *(2^16)*         | 1,073,741,824 *(2^30)*  |
| Class C | `192.0.0.0` - `223.255.255.255` | 2,097,152 *(2^21)*   | 256 *(2^8)*             | 536,870,912 *(2^29)*    |
| Class D | `224.0.0.0` - `239.255.255.255` | *Undefined*          | *Undefined*             | 268,435,456 *(2^28)*    |
| Class E | `240.0.0.0` - `255.255.255.255` | *Undefined*          | *Undefined*             | 268,435,456 *(2^28)*    |

### IPv4 Address Binary Code Representation
| *Class*        | *Dot-Decimal Notation* | *Binary Code*                         |
| :------------: | :--------------------: | :-----------------------------------: |
| Class A        | `0.0.0.0`              | `00000000.00000000.00000000.00000000` |
|                | `127.255.255.255`      | `01111111.11111111.11111111.11111111` |
|                |                        | `0nnnnnnn.HHHHHHHH.HHHHHHHH.HHHHHHHH` |
|                |                        |                                       |
| Class B        | `128.0.0.0`            | `10000000.00000000.00000000.00000000` |
|                | `191.255.255.255`      | `10111111.11111111.11111111.11111111` |
|                |                        | `10nnnnnn.nnnnnnnn.HHHHHHHH.HHHHHHHH` |
|                |                        |                                       |
| Class C        | `192.0.0.0`            | `11000000.00000000.00000000.00000000` |
|                | `223.255.255.255`      | `11011111.11111111.11111111.11111111` |
|                |                        | `110nnnnn.nnnnnnnn.nnnnnnnn.HHHHHHHH` |
|                |                        |                                       |
| Class D        | `224.0.0.0`            | `11100000.00000000.00000000.00000000` |
|                | `239.255.255.255`      | `11101111.11111111.11111111.11111111` |
|                |                        | `1110XXXX.XXXXXXXX.XXXXXXXX.XXXXXXXX` |
|                |                        |                                       |
| Class E        | `240.0.0.0`            | `11110000.00000000.00000000.00000000` |
|                | `255.255.255.255`      | `11111111.11111111.11111111.11111111` |
|                |                        | `1111XXXX.XXXXXXXX.XXXXXXXX.XXXXXXXX` |
- `n` indicates a bit used for the network ID
- `H` indicates a bit used for the host ID
- `X` indicates a bit without a specified purpose

### Private IPv4 Addresses
1. `10.0.0.0` - `10.255.255.255`
2. `172.16.0.0` - `172.31.255.255`
3. `192.168.0.0` - `192.168.255.255`

## Netmask
- 32-bit binary mask used to divide an IPv4 address into subnets
- First address in the subnet is the assigned network address *(all-bits-zero host value)*
- Last address in the subnet is the assigned broadcast address *(all-bits-one host value)*

### IPv4 Address Default Netmask
| *Class* | *Default Netmask*       | *CIDR Notation* |
| :-----: | :---------------------: | :-------------: |
| Class A | `255.0.0.0`             | `/8`            |
| Class B | `255.255.0.0`           | `/16`           |
| Class C | `255.255.255.0`         | `/24`           |
| Class D | *Undefined*             | *Undefined*     |
| Class E | *Undefined*             | *Undefined*     |

### Example 1: `10.21.145.137/13`
|                | *Dot-Decimal Notation* | *Binary Code*                           |
| :------------: | :--------------------: | :-------------------------------------: |
| Address        | `10.21.145.137`        | `00001010.00010  101.00101101.10001001` |
| Netmask        | `255.248.0.0`          | `11111111.11111  000.00000000.00000000` |
| Network        | `10.16.0.0`            | `00001010.00010  000.00000000.00000000` |
| HostMin        | `10.16.0.1`            | `00001010.00010  000.00000000.00000001` |
| HostMax        | `10.23.255.254`        | `00001010.00010  111.11111111.11111110` |
| Broadcast      | `10.23.255.255`        | `00001010.00010  111.11111111.11111111` |
| Next Network   | `10.24.0.0`            | `00001010.00011  000.00000000.00000000` |

- `10.21.145.137/13` belongs to the subnet `10.16.0.0` - `10.23.255.255`
- Number of hosts in the subnet = *524286 (2^19 - 2)*

### Example 2: `156.67.154.75/28`
|                | *Dot-Decimal Notation* | *Binary Code*                           |
| :------------: | :--------------------: | :-------------------------------------: |
| Address        | `156.67.154.75`        | `10011100.01000011.10011010.0100  1011` |
| Netmask        | `255.255.255.240`      | `11111111.11111111.11111111.1111  0000` |
| Network        | `156.67.154.64`        | `10011100.01000011.10011010.0100  0000` |
| HostMin        | `156.67.154.65`        | `10011100.01000011.10011010.0100  0001` |
| HostMax        | `156.67.154.78`        | `10011100.01000011.10011010.0100  1110` |
| Broadcast      | `156.67.154.79`        | `10011100.01000011.10011010.0100  1111` |
| Next Network   | `156.67.154.80`        | `10011100.01000011.10011010.0101  0000` |

- `156.67.154.75/28` belongs to the subnet `156.67.154.64` - `156.67.154.79`
- Number of hosts in the subnet = *14 (2^4 - 2)*

## Transmission Control Protocol (TCP)
- Connection-oriented protocol
- Does not support broadcasting
- Comparatively slower than UDP
- Reliable and guarantees delivery of data to destination
- Sequences data; packets arrive in-order at the receiver
- Extensive error checking mechanism; provides flow control & acknowledgment of data

## User Datagram Protocol (UDP)
- Datagram-oriented protocol
- Supports broadcasting
- Faster, simpler & more efficient than TCP
- Delivery of data to destination cannot be guaranteed in UDP
- No sequencing of data; has to be managed by application layer if ordering is required
- Only has basic error checking mechanism using checksums

## Open Systems Interconnection (OSI) Model
1. Application Layer
2. Presentation Layer
3. Session Layer
4. Transport Layer
5. Network Layer
6. Data Link Layer
7. Physical Layer

## Dynamic Host Configuration Protocol (DHCP)
- Used for both IPv4 & IPv6
- Uses UDP as its transport protocol
- Automates IP configuration, including IP address, subnet mask, default gateway & DNS information

## Domain Name System (DNS)
- Translates internet domain and host names to IP address

## Ping
- Operates by sending *Internet Control Message Protocol (ICMP)* echo request packets to target host and waiting for ICMP echo reply
- `ping localhost` or `ping 127.0.0.1` to test IP stack
