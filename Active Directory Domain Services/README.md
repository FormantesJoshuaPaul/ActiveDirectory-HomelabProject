Active Directory Domain Services

This portion is to build and administer a small Active Directory environment using Windows Server 2025.
The objective is to gain practical experience with Active Directory Domain Services, DNS, Domain Controllers, identity management, and troubleshooting in an enterprise-style environment.

Here I am installing Active Directory Domain Services and promoting the Windows Server 2025 (DC01) to be a domain controller for the new domain forest.


Domain settings/configuration:
- Domain: adlab.test
- NETBIOS domain name: ADLAB
- Domain Controller: DC01
- FQDN: DC01.adlab.test
- Forest: adlab.test
- Global Catalog: Enabled
- Read-only Domain Controller: No
- DNS Server: Enabled

Domain Controller Options

- DNS Server: Enabled
- Global Catalog: Enabled
- Read-Only Domain Controller: Disabled
- Forest functional level: Windows Server 2025
- Domain functional level: Windows Server 2025

AD Database and SYSVOL

Default paths were used:

- AD database: C:\Windows\NTDS
- AD log files: C:\Windows\NTDS
- SYSVOL: C:\Windows\SYSVOL

Services Enabled:
- Active Directory Domain Services
- DNS Server
- Global Catalog
- Netlogon

Verified deployment using PowerShell:
- Get-ADDomain
This confirmed:

Domain: adlab.test
NetBIOS name: ADLAB
Forest: adlab.test
DC01 as the current domain infrastructure server

- Get-ADDomainController
This confirmed:

Hostname: DC01.adlab.test
IPv4 address: 192.168.56.10
Global Catalog: Enabled
Read-only: False

- Get-Service DNS
- Get-Service NTDS
- Get-Service Netlogon
The DNS Server, Active Directory Domain Services, and Netlogon
services were confirmed to be running.

DNS

DNS was installed and configured as part of the Domain Controller deployment. 

The following AD DNS zones were created:
- adlab.test
- _msdcs.adlab.test

Tested DNS resolution for adlab.test domain:
- nslookup adlab.test
- nslookup dc01.adlab.test

Active Directory Service Discovery

An SRV lookup was used to verify that DNS could locate the Domain
Controller's LDAP service:
_ldap._tcp.dc._msdcs.adlab.test

The lookup successfully returned:

dc01.adlab.test

** Troubleshooting: Multi-NIC DNS Registration Problem

DC01 was configured with two network interfaces:

Interface	Network	Address	Purpose
Ethernet	NAT	10.0.2.15	Internet access
Ethernet 2	Host-only	192.168.56.10	Private AD network

Domain-Joined CLIENT01

A Windows 11 Pro virtual machine CLIENT01 was configured on the private 192.168.56.0/24 network and configured to use DC01 192.168.56.10 as its DNS server.

The client was successfully joined to the adlab.test domain.

CLIENT01 Configuration

- Hostname: CLIENT01
- IP address: 192.168.56.20
- DNS server: 192.168.56.10 (DC01)
- Domain: adlab.test

Validation

The domain join was verified from the client using powershell commands:

- `whoami`
- `Get-CimInstance Win32_ComputerSystem | Select-Object Name,Domain,PartOfDomain`

It was then verified on DC01's side by checking whether CLIENT01 exists under 
Server Manager > Tools > Active Directory Users and Computers > Computers folder

User Authentication

A test domain user was authenticated against the adlab.test domain from the domain-joined Windows 11 client.

The account was verified using powershell commands:
- `whoami`
- `whoami /groups`

Lessons Learned

This exercise provided hands-on experience with:

Deploying Active Directory Domain Services
Promoting a Windows Server to a Domain Controller
Understanding domains and forests
Understanding NetBIOS names and FQDNs
Understanding DNS zones and DNS records
Understanding A and SRV records
Understanding the relationship between DNS and AD DS
Troubleshooting DNS registration on a multi-NIC Domain Controller
Using PowerShell to inspect and modify Windows Server configuration
Validating configuration changes instead of relying on assumptions

  
