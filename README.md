# PNPT-PREPARATION

# Active Directory Home Lab for PNPT Preparation - Phase 1

## Overview

This project documents the creation of a realistic Active Directory home lab designed to support preparation for the Practical Network Penetration Tester (PNPT) certification.

The goal was to build an environment that closely resembles a small enterprise network where both defensive and offensive security skills can be practiced.

The lab includes:

* Windows Server Domain Controller (DC01)
* Windows 11 Domain-Joined Workstation (WIN11-CLIENT)
* Kali Linux Attacker Machine
* Ubuntu Wazuh Server
* Active Directory Domain Services (AD DS)
* DNS
* Organizational Units (OUs)
* Security Groups
* Shared Folders
* Simulated Enterprise Users

---

## Lab Architecture

### Virtual Machines

| Machine      | Role               | IP Address     |
| ------------ | ------------------ | -------------- |
| DC01         | Domain Controller  | 192.168.17.10  |
| WIN11-CLIENT | Domain Workstation | DHCP           |
| Kali Linux   | Attacker Machine   | 192.168.17.101 |
| Ubuntu Wazuh | SIEM Platform      | DHCP           |

### Network Design

Kali Linux uses two network adapters:

* Host-Only Adapter (VMnet1) for attacking the lab
* NAT Adapter for internet access

All Windows systems remain isolated inside the Host-Only network.

This design allows safe internal testing while still enabling Kali to install and update tools.

---

## Active Directory Configuration

### Domain

corp.local

### Organizational Units

* Users
* Workstations
* Servers
* Service Accounts
* Admins

### Users Created

| Name           | Username   |
| -------------- | ---------- |
| John J Smith   | j.smith    |
| Mary J Jones   | m.jones    |
| IT Admin       | it.admin   |
| Help Desk      | helpdesk   |
| Backup Service | svc_backup |

### Security Groups

* HR
* FINANCE
* IT
* BACKUPOPERATORS

### Group Memberships

Users were assigned to department groups to simulate a real enterprise environment.

---

## File Shares

The following departmental shares were created:

* HR
* FINANCE
* IT
* PUBLIC

Permissions were configured using Active Directory security groups.

The PUBLIC share was configured with read access for all authenticated users.

---

## Challenges Encountered

### 1. Domain Authentication Failure

After joining the workstation to the domain, users appeared unable to log in using domain credentials.

Investigation revealed that the workstation was still authenticating against a local account rather than Active Directory.

Validation commands:

whoami

echo %logonserver%

Lessons learned:

* Local and domain accounts can have similar names but behave differently.
* Verifying authentication context is critical.

---

### 2. Domain Controller Availability

During testing, the Domain Controller was powered off.

This resulted in:

* Authentication failures
* DNS resolution issues
* Inability to validate domain credentials

Lessons learned:

* Active Directory environments are highly dependent on Domain Controllers.
* DNS and authentication services are tightly coupled.

---

### 3. Active Directory User Discovery Issues

PowerShell queries initially returned unexpected results when enumerating users.

The issue was caused by querying incorrect attributes and misunderstanding how Active Directory stores usernames.

Lessons learned:

* Display names and SamAccountNames are different objects.
* Enumeration requires understanding of AD schema attributes.

---

### 4. Network Architecture Decisions

A significant design decision involved choosing between:

* Bridged Networking
* Host-Only Networking
* NAT Networking

Final decision:

* Host-Only for lab systems
* NAT for Kali internet access

Lessons learned:

* Isolation improves safety during offensive testing.
* NAT does not hide traffic from the ISP.
* Host-Only networks prevent accidental interaction with production networks.

---

## Key Skills Learned

* Active Directory deployment
* Domain joining systems
* DNS troubleshooting
* User and group management
* Organizational Unit design
* Windows share permissions
* VMware network architecture
* Authentication troubleshooting
* Basic enterprise network design

---

## Phase 1 Outcome

Successfully built a functional Active Directory environment suitable for:

* PNPT preparation
* Active Directory enumeration
* BloodHound analysis
* SMB enumeration
* Kerberos attacks
* Wazuh monitoring
* SOC detection engineering

Phase 2 will focus on Active Directory enumeration and internal penetration testing techniques from Kali Linux.
