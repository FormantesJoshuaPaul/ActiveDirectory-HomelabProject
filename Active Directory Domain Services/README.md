Active Directory Domain Services

Installing Active Directory Domain Services and configure the first domain controller for a new Active Directory forest.

Domain settings/configuration:
- Domain: adlab.test
- NETBIOS domain name: ADLAB
- Domain Controller: DC01
- FQDN: DC01.adlab.test
- Forest: adlab.test
- Global Catalog: Enabled
- Read-only Domain Controller: No
- DNS Server: Enabled

Services:
- Active Directory Domain Services
- DNS
- Global Catalog

Verified deployment using PowerShell:
- Get-ADDomain
- Get-ADDomainController
- Get-Service DNS
- Get-Service NTDS

Tested DNS resolution for adlab.test domain:
- nslookup adlab.test
  
