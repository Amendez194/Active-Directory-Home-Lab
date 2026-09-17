# Active Directory Home Lab

## Overview

I built this home lab to gain hands-on experience with Windows Server, Active Directory, domain environments, Group Policy, and network file permissions.

The lab simulates a small business environment using Windows Server 2025 as a domain controller and a Windows 11 workstation as a domain-joined client.

Rather than only installing Active Directory, I configured the environment, created users and security groups, joined a workstation to the domain, applied Group Policy, configured a network file share, and troubleshot an access issue.

## Lab Environment

**Hypervisor:** Oracle VirtualBox

**Domain Controller**
- Windows Server 2025
- Hostname: DC01
- IP Address: 192.168.50.10
- Roles: Active Directory Domain Services, DNS, File Server

**Client**
- Windows 11 Enterprise
- Hostname: CLIENT01
- IP Address: 192.168.50.20
- Domain joined to adlab.local

**Virtual Network**
- VirtualBox Internal Network: LABNET

## Technologies and Skills

- Windows Server 2025
- Windows 11 Enterprise
- Active Directory Domain Services (AD DS)
- DNS
- Organizational Units (OUs)
- Active Directory users and security groups
- Group Policy
- Domain authentication
- NTFS permissions
- Windows file sharing
- TCP/IP configuration
- DNS and network connectivity testing
- Windows command-line troubleshooting

## Active Directory Configuration

I created the `adlab.local` Active Directory forest and promoted DC01 to a domain controller.

I then created organizational units to represent different areas of the simulated company and created domain users and security groups.

Permissions were assigned through security groups rather than directly to individual users to practice a more scalable method of access management.

## Domain Client

I configured CLIENT01 to use the domain controller for DNS and verified network communication between the client and DC01.

CLIENT01 was then joined to the `adlab.local` domain.

I verified successful domain authentication by logging into CLIENT01 with a domain user and using:

`whoami`

## Group Policy

I used Group Policy Management on DC01 to configure policies for domain users.

I refreshed and verified Group Policy from the client using commands including:

`gpupdate /force`

`gpresult /r`

## File Share and Permissions

I created an Employees network share on DC01:

`\\DC01\Employees`

Access was configured using a combination of share permissions and NTFS permissions assigned to the `GG_Employees` security group.

I tested the share from CLIENT01 while authenticated as a domain employee.

## Troubleshooting Incident

During testing, the domain user `jlee` received an access-denied error when attempting to access the Employees network share.

Instead of immediately changing the folder permissions, I verified the logged-in user and checked the user's current security groups with:

`whoami /groups`

I discovered that `GG_Employees` was not present in the user's current security token.

I verified the user's group membership in Active Directory Users and Computers and refreshed the user's logon session. After signing back into CLIENT01, I verified the group membership again and successfully accessed the network share.

This demonstrated how Active Directory group membership, Windows security tokens, share permissions, and NTFS permissions work together when determining access to network resources.

## Troubleshooting Commands Used

- `ipconfig /all` — Verify IP and DNS configuration
- `ping 192.168.50.10` — Test connectivity to the domain controller
- `nslookup adlab.local` — Verify DNS resolution
- `whoami` — Verify the authenticated user
- `whoami /groups` — Verify security group membership
- `gpupdate /force` — Refresh Group Policy
- `gpresult /r` — View applied Group Policy

## What I Learned

This project helped me understand how several Windows enterprise technologies work together rather than treating them as separate concepts.

In particular, I gained hands-on experience with the relationship between DNS and Active Directory, centralized domain authentication, security groups and permissions, Group Policy, and systematic troubleshooting of user access problems.
