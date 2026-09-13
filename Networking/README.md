Networking portion of the homelab

Objective

Establish an isolated network for the Active Directory lab while maintaining internet connectivity for the Windows Server VM.

Network Configuration
Ethernet: NAT | 10.0.2.15 | Internet Access |
Ethernet 2: Host-only | 192.168.56.10 | Private lab network

Network design

The NAT adapter will provide internet connectivity for our domain controller DC01.
The Host-only adapter will provide an isolated network for DC01 and future virtual machines such as CLIENT01.

Static Addressing

The Host-only interface was configured with:
IP: 192.168.56.10
Subnet mask: 255.255.255.0
Gateway: None

Static IP address to make sure that clients VMs can locate the Domain Controller consistently, rather than using DHCP to automatically assign a IP address.

Testing

Connectivity to the VirtualBox NAT gateway was tested with:
powershell
ping 10.0.2.2

