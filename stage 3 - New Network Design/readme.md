# ByBank – Secure Network Redesign (Stage 3)

## Overview

This project demonstrates a **secure network architecture redesign for a fictional financial institution (ByBank)**.

The goal of this stage is to transform a **flat, insecure network** into a **segmented, defense-in-depth architecture** that protects critical banking assets and customer data.

This lab environment was built using **GNS3, VMware, pfSense, Active Directory, and Linux servers** to simulate a realistic enterprise banking infrastructure.

---

# 🎯 Objectives

The redesigned network aims to:

- Eliminate **flat network architecture**
- Implement **network segmentation**
- Enforce **least privilege access**
- Reduce **lateral movement during attacks**
- Protect **critical banking systems**
- Align with **security best practices used in financial institutions**

---

# 🏗 Network Architecture

The network is segmented into **three major security zones**:

## External Zone
- Interface between the **internet and the organization**
- Acts as a **buffer layer**
- Controlled by the **edge firewall**

## DMZ (Demilitarized Zone)
- Hosts **public-facing services**
- Allows customers to access banking services
- Isolated from internal infrastructure

Example services:

- Web server

## Internal Zone

Contains **critical banking infrastructure** and is segmented using **VLANs**.

Internal segmentation includes:

- Server Network
- Security Operations Network
- IT Administration Network
- Developer Network
- Finance Department Network

This segmentation prevents attackers from easily moving across departments.

---

# 🔐 Security Design Principles

The network redesign follows core **enterprise security principles**.

## Network Segmentation
Departments are separated using **VLANs** and firewall policies.

## Least Privilege Access
Users and departments only have access to **resources required for their role**.

## Defense in Depth
Multiple layers of security including:

- Firewall controls
- Network segmentation
- Endpoint monitoring
- Centralized logging

## Monitoring and Detection

Security tools are integrated for visibility and threat detection.

- **Splunk** for centralized logging
- **Wazuh** for endpoint detection and monitoring

---

# 🖥 Lab Infrastructure

The environment was built using a **hybrid GNS3 + VMware lab**.

## GNS3 Components

- Cisco Router (Internal / External routing)
- Cisco Switch (VLAN segmentation)

## VMware Virtual Machines

| Machine | OS | Purpose |
|------|------|------|
| BYBANKWINSRV | Windows Server 2019 | Active Directory / Domain Services |
| BYBANKDB | Ubuntu | Database Server |
| BYBANKWeb | Ubuntu | Web Server (DMZ) |
| BYBANKSplunk | Ubuntu | Centralized Log Management |
| BYBANKWazuh | Ubuntu | Endpoint Detection & Security |
| BYBANKAnsibleCtrl | Ubuntu | Linux Configuration Management |
| BYBANKSEC01 | Kali Linux | Security Testing |
| BYBANKIT01 | Windows 11 | IT Department Workstation |
| BYBANKFIN01 | Windows 11 | Finance Workstation |
| BYBANKDEV01 | Windows 11 | Developer Workstation |
| BYBANKSAL01 | Windows 11 | Sales Workstation |

---

# 🔧 Technologies Used

| Technology | Purpose |
|------|------|
| Cisco Router | Internal and external network routing |
| Cisco Switch | VLAN segmentation |
| pfSense Firewall | Zone protection and traffic control |
| Active Directory | Identity and access management |
| Ubuntu Servers | Hosting services and monitoring tools |
| Windows 11 Clients | Department workstations |
| Splunk | Centralized logging and monitoring |
| Wazuh | Endpoint detection and response |
| Ansible | Linux configuration management |

---

# 🌐 Network Subnets

| Zone | VLAN | Subnet |
|------|------|------|
| External | — | 192.168.10.0/24 |
| DMZ | — | 192.168.15.0/24 |
| Server Network | VLAN 1001 | 192.168.35.0/24 |
| Security Network | VLAN 1010 | 192.168.30.0/24 |
| IT Department | VLAN 1015 | 192.168.50.0/24 |
| Developer Network | VLAN 1025 | 192.168.45.0/24 |
| Finance Network | VLAN 1020 | 192.168.55.0/24 |

---

# 🔑 Identity and Access Management

All endpoints were joined to **Active Directory** for centralized identity management.

Linux machines were integrated with AD using:

- `realm`
- `sssd`

Windows endpoints were joined directly to the domain.

A **DHCP server** was configured to automatically assign IP addresses.

---

# 🔐 Example User Accounts

| Department | Username | Full Name | Role |
|------|------|------|------|
| Security | sec-jmiller | Jordan Miller | Network Security |
| Security | sec-aharper | Alex Harper | Security Analyst |
| Sales | sls-rgupta | Rajesh Gupta | Customer Representative |
| Sales | sls-lchen | Lisa Chen | Senior Sales Executive |
| Finance | fin-asobel | Anita Sobel | Finance Analyst |
| Finance | fin-mbecker | Marcus Becker | Finance Manager |
| IT | it-dsmith | David Smith | IT Technician |
| IT | it-kpatel | Kavita Patel | IT Manager |
| Developer | dev-smehta | Sanjay Mehta | Lead Developer |
| Developer | dev-jchoi | Julia Choi | Junior Developer |

---

# 🔥 Firewall Architecture

A **pfSense Next Generation Firewall** is deployed as the primary security gateway.

Responsibilities include:

- Inter-VLAN traffic filtering
- DMZ protection
- External access control
- Network logging

At the current stage, firewall rules remain **open for troubleshooting and testing**.  
Future stages will implement **strict rule-based segmentation**.

---

# 📊 Monitoring & Security Operations

## Splunk

Used for:

- Centralized log ingestion
- Network monitoring
- Security event correlation

## Wazuh

Provides:

- Endpoint monitoring
- File integrity monitoring
- Intrusion detection
- Security alerting

---

# ⚙ Configuration Management

**Ansible** is used to manage Linux systems for:

- Automated configuration
- Security hardening
- Package management
- Consistent server setup

---

# 🖼 Architecture Diagrams

## Network Design

![image](https://github.com/jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/blob/main/stage%203%20-%20New%20Network%20Design/Screenshots/Network%20Design.png?raw=true)

## GNS3 + VMware Integration
![image](https://github.com/jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/blob/main/stage%203%20-%20New%20Network%20Design/Screenshots/GNS3.png?raw=true)

![image](https://github.com/jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/blob/main/stage%203%20-%20New%20Network%20Design/Screenshots/VMWare.png?raw=true)

---

# 🚀 Future Improvements

Planned improvements for upcoming stages:

- Implement **strict firewall rules**
- Deploy **IDS / IPS monitoring**
- Enable **SIEM detection rules**
- Simulate **attack scenarios and incident response**
- Implement **Zero Trust network access policies**

---

# 📚 Project Context

This project is part of a **multi-stage cybersecurity lab focused on enterprise security architecture, monitoring, and attack detection**.

Future stages will include:

- Hardening the network using existing benchmarks and threats.
