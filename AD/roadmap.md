
# Active Directory (AD) Pentesting Roadmap

---

# Stage 1: Networking Fundamentals

## Learn

- OSI Model
- TCP/IP Model
- IP Addressing
- Subnetting
- ARP
- DNS
- DHCP
- NAT
- Routing
- VLANs
- VPNs
- TCP
- UDP
- SMB
- LDAP
- Kerberos
- RDP
- SSH

## Tools

### Wireshark

Learn:

- Packet Capture
- TCP Handshake
- DNS Analysis
- HTTP/HTTPS Traffic
- Kerberos Traffic
- SMB Traffic
- NTLM Authentication

### Nmap

Learn:

- Host Discovery
- Port Scanning
- Service Detection
- OS Detection
- NSE Scripts

---

# Stage 2: Windows Fundamentals

## Learn

- Windows Architecture
- Registry
- Services
- Processes
- Threads
- DLLs
- Event Logs
- NTFS Permissions
- UAC
- PowerShell

## Tools

### Process Explorer

Learn:

- Parent and Child Processes
- Running Services
- Process Investigation

### Autoruns

Learn:

- Startup Entries
- Persistence Mechanisms

### Event Viewer

Learn:

- Login Events
- Security Events
- Service Events
- PowerShell Logs

---

# Stage 3: Active Directory Fundamentals

## Learn

### Core Components

- Domain
- Forest
- Tree
- Domain Controller (DC)
- Organizational Units (OU)
- Users
- Groups
- Computers
- Group Policy Objects (GPO)
- Trusts

### DNS and Active Directory

Learn:

- Why AD Requires DNS
- Service Records
- Domain Discovery

## Understand

```text
User → Domain → Domain Controller
```

## Be Able To Explain

- What is Active Directory?
- What is a Domain Controller?
- What is a Forest?
- What is a Trust?
- What is a GPO?

---

# Stage 4: Authentication

## NTLM

Learn

- NTLM Hashes
- Challenge-Response Authentication
- NTLM Authentication Flow

## Kerberos

Learn

- KDC
- Authentication Server (AS)
- Ticket Granting Server (TGS)
- TGT
- TGS Ticket
- SPN
- Delegation

## Be Able To Draw

```text
User
 ↓
Authentication Server
 ↓
TGT
 ↓
Ticket Granting Server
 ↓
Service Ticket
 ↓
Application Access
```

---

# Stage 5: Active Directory Enumeration

## Goal

Discover everything inside the domain.

## Tools

### PowerView

Learn:

- User Enumeration
- Group Enumeration
- Computer Enumeration
- Share Enumeration
- ACL Enumeration
- Domain Admin Discovery

Example Commands:

```powershell
Get-DomainUser
Get-DomainGroup
Get-DomainComputer
Get-DomainTrust
```

### BloodHound

Learn:

- Nodes
- Edges
- ACLs
- Attack Paths
- Privilege Escalation Paths

Be Able To Answer:

- How can User A become Domain Admin?
- Which permissions are dangerous?
- Which machines are vulnerable?

### SharpHound

Learn:

- User Collection
- Group Collection
- ACL Collection
- Session Collection
- Trust Collection

Understand exactly what data is collected.

---

# Stage 6: Windows Privilege Escalation

## Learn

- Service Misconfigurations
- Weak Permissions
- Unquoted Service Paths
- Scheduled Tasks
- Credential Exposure
- UAC Bypass Concepts
- Token Abuse

## Tools

### WinPEAS

Learn:

- Privilege Escalation Checks
- Service Weaknesses
- Sensitive Files
- Misconfigurations

### Seatbelt

Learn:

- User Information
- Installed Software
- Security Settings
- Interesting Files
- Privileges

---

# Stage 7: Credential Attacks

## Learn

- Password Spraying
- Password Cracking
- Credential Dumping
- Hashes
- Tickets

## Tools

### Mimikatz

Learn:

- LSASS
- Password Extraction Concepts
- Hash Extraction Concepts
- Kerberos Tickets

Understand:

- Pass-the-Hash
- Pass-the-Ticket

### Rubeus

Learn:

- Kerberoasting
- Ticket Requests
- Ticket Extraction
- Delegation Abuse

### Responder

Learn:

- LLMNR
- NBT-NS
- Name Resolution Attacks
- Hash Capture

---

# Stage 8: Lateral Movement

## Learn

- SMB
- WMI
- WinRM
- RDP
- PsExec Concepts

## Tools

### Impacket

Learn:

- SMB Authentication
- Remote Execution Concepts
- NTLM Authentication

### NetExec

Learn:

- Host Enumeration
- User Enumeration
- Share Enumeration
- Credential Validation

---

# Stage 9: Advanced Active Directory Attacks

## Learn

### Kerberoasting

Understand:

- SPNs
- Service Accounts
- Ticket Cracking Concepts

### AS-REP Roasting

Understand:

- Pre-Authentication
- User Misconfigurations

### Pass-the-Hash

Understand:

- NTLM Authentication
- Hash Reuse

### Pass-the-Ticket

Understand:

- Kerberos Ticket Abuse

### DCSync

Understand:

- AD Replication
- Replication Rights

### Golden Ticket

Understand:

- KRBTGT Account
- Forged TGTs

### Silver Ticket

Understand:

- Forged Service Tickets

### Delegation Attacks

Learn:

- Unconstrained Delegation
- Constrained Delegation
- Resource-Based Constrained Delegation (RBCD)

---

# Stage 10: Active Directory Certificate Services (AD CS)

## Learn

### PKI Fundamentals

- Certificates
- Certificate Authorities
- Certificate Templates
- Enrollment Rights

### Attack Paths

- ESC1
- ESC2
- ESC3
- ESC4
- ESC6
- ESC8
- Shadow Credentials
- PKINIT

## Tools

### Certipy

Learn:

- Enumeration
- Vulnerable Templates
- Certificate Abuse

### Certify

Learn:

- Template Discovery
- Enrollment Permissions

---

# Stage 11: Detection and Defense

## Learn

- Windows Event Logs
- Event IDs
- Sysmon
- Threat Hunting
- Detection Engineering
- Incident Response Basics

## Tools

### Sysmon

Learn:

- Process Creation Logs
- Network Connections
- File Monitoring

### Splunk

Learn:

- Log Searching
- Correlation
- Dashboards
- Detection Rules

### Microsoft Sentinel

Learn:

- KQL
- Analytics Rules
- Hunting Queries
- Incident Investigation

---

# Stage 12: Reporting

## Learn

### Executive Summary

Explain findings to management.

### Technical Findings

Include:

- Vulnerability Description
- Impact
- Evidence
- Reproduction Steps
- Remediation

### Risk Assessment

Learn:

- CVSS
- Risk Ratings
- Business Impact

---

# Labs To Complete

## TryHackMe

- Pre Security
- Network Fundamentals
- Windows Fundamentals
- Active Directory Basics
- Attacktive Directory

## Hack The Box

- Active Directory Machines
- Pro Labs

## GOAD

- Game of Active Directory

## DetectionLab

- Blue Team Lab

---

# Final Mastery Checklist

You should be able to explain without notes:

- DNS in Active Directory
- Kerberos Authentication
- NTLM Authentication
- BloodHound Attack Paths
- GPOs
- Trust Relationships
- Privilege Escalation
- Credential Attacks
- Lateral Movement
- DCSync
- Golden Ticket
- Silver Ticket
- Delegation Attacks
- AD CS Abuse
- Detection Techniques
- Professional Pentest Reporting

---

# Learning Order

1. Networking
2. Windows Fundamentals
3. Active Directory Fundamentals
4. Authentication (NTLM + Kerberos)
5. AD Enumeration
6. Windows Privilege Escalation
7. Credential Attacks
8. Lateral Movement
9. Advanced AD Attacks
10. AD CS
11. Detection & Defense
12. Reporting

---

# Goal

Become capable of performing:

- Enterprise Active Directory Assessments
- Internal Network Penetration Tests
- Red Team Operations
- Active Directory Security Reviews
- Bank and Enterprise Environment Testing

