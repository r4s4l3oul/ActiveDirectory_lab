# Active Directory Home Lab

## Overview

This project documents the creation of a complete Active Directory lab environment for learning Windows administration and Active Directory security.

## Objectives

- Deploy a Windows Server 2022 Domain Controller
- Configure Active Directory Domain Services (AD DS)
- Manage users and groups
- Configure Group Policy Objects (GPO)
- Configure SMB shares
- Join a Windows 11 workstation to the domain
- Perform Active Directory security auditing
- Learn BloodHound and PingCastle

## Planned Infrastructure

```text
DC01 (Windows Server 2022)
│
├── Active Directory
├── DNS
├── SMB
└── GPO

CLIENT01 (Windows 11)
│
└── Domain Joined Workstation

Kali Linux
│
└── Security Auditing
```

## Technologies

- Windows Server 2022
- Active Directory
- DNS
- SMB
- Group Policy
- Windows 11
- Kali Linux
- BloodHound
- PingCastle

## Progress

- [x] Download Windows Server 2022
- [x] Create DC01 virtual machine
- [x] Install Windows Server 2022
- [x] Configure static IP
- [x] Install Active Directory Domain Services
- [x] Create LAB.LOCAL domain
- [x] Create users and groups
- [ ] Configure GPOs
- [ ] Configure SMB shares
- [x] Join CLIENT01 to the domain
- [ ] BloodHound analysis
- [ ] PingCastle audit

## Author

Anass Moumen

Cybersecurity Engineering Student – EFREI Paris
