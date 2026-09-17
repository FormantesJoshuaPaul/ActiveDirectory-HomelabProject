Active Directory Homelab

Overview

This project will be a hands-on Active Directory IT homelab designed to develop practical skills in systems administration, identity and access management, endpoint management, networking, security and troubleshooting.

Objectives

The project will aim to provide practical experience with:
- AD DS
- DNS and DHCP
- Windows Server Administration
- Group Policy
- Powershell
- Identity and access management
- Endpoint management
- Networking
- Troubleshooting
- User onboarding and offboarding
- IT documentation

  Planned environment

  On-Premises
  - Windows Server 2025
  - Windows 11
  - Oracle VirtualBox
  - Active Directory Domain Services
  - DNS
  - DHCP
 
  Project status
  ** In Progress **

  Current architecture:
  
                    adlab.test
                         │
                  ┌──────▼──────┐
                  │    DC01     │
                  │ AD DS + DNS │
                  │192.168.56.10│
                  └──────┬──────┘
                         │
                  Host-only LAN
                         │
                  ┌──────▼──────┐
                  │  CLIENT01   │
                  │ Windows 11  │
                  │192.168.56.20│
                  └─────────────┘

  
 
