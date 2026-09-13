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

Network configuration:
DC01 uses two network interfaces
Ethernet: NAT | 10.0.2.15 | For internet connectivity 
Ethernet 2: Host Only | 192.168.56.10 | Private Active Directory lab network 

The first ethernet adapter is used solely for internet connectivity from the domain controller, and the second ethernet adapter is used for the private Active Directory network, isolating the client VMs from having internet access.

Services Enabled:
- Active Directory Domain Services
- DNS Server
- Global Catalog
- Netlogon

Verified deployment using PowerShell:
- Get-ADDomain
- Get-ADDomainController
- Get-Service DNS
- Get-Service NTDS
- Get-Service Netlogon

DNS

DNS was installed and configured as part of the Domain Controller deployment. 

The following AD DNS zones were created:
- adlab.test
- _msdcs.adlab.test

Tested DNS resolution for adlab.test domain:
- nslookup adlab.test
- nslookup dc01.adlab.test
  
