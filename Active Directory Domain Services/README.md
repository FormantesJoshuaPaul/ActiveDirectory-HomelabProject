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

After AD DS deployment, the DNS record for:

dc01.adlab.test

contained both IPv4 addresses:

192.168.56.10
10.0.2.15

The intended AD communication path was the private
192.168.56.0/24 Host-only network.

The NAT address was therefore an unwanted address in the
Domain Controller's DNS record.

Investigation
1. Checked DNS Client Registration

The DNS registration settings for both network interfaces were
checked using:

Get-DnsClient |
Select-Object InterfaceAlias,
InterfaceIndex,
RegisterThisConnectionsAddress,
UseSuffixWhenRegistering

Both network interfaces were initially configured to allow their
addresses to be registered in DNS.

2. Inspected the DNS Record

The DNS record for DC01 was inspected using:

Get-DnsServerResourceRecord 
-ZoneName "adlab.test" 
-Name "dc01"

The results showed both:

192.168.56.10
10.0.2.15
3. Tested DNS Re-registration

The Domain Controller's DNS registration was refreshed using:

nltest /dsregdns

The unwanted 10.0.2.15 record returned.

This showed that manually removing the record was not enough and
that the underlying DNS registration behaviour needed to be addressed.

4. Inspected DNS Server Configuration

The DNS Server listening interfaces were inspected using:

Get-DnsServerSetting

The DNS Server was listening on both network interfaces.

The adlab.test zone's name server configuration was also inspected
to confirm that dc01.adlab.test was associated with the intended
AD network address.

Resolution

The NAT interface was configured not to register its address in DNS:

Set-DnsClient 
-InterfaceAlias "Ethernet" 
-RegisterThisConnectionsAddress $false

The DNS Server was then configured to listen on the intended
Active Directory interface:

192.168.56.10

The adlab.test zone was verified to use:

dc01.adlab.test
192.168.56.10

The stale 10.0.2.15 A record was removed.

Validation

After the configuration changes, DNS registration was forced again:

nltest /dsregdns

The DNS record for DC01 was checked again:

Get-DnsServerResourceRecord
-ZoneName "adlab.test" 
-Name "dc01"

The final result contained:

dc01.adlab.test → 192.168.56.10

The unwanted 10.0.2.15 record did not return.

DNS resolution was then verified with:

nslookup dc01.adlab.test

The result resolved DC01 to the intended private lab address.

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

  
