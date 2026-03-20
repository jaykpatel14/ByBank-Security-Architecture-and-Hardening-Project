## 🎯 Objective
This project demonstrates hands-on implementation of **pfSense firewall hardening** based on CIS Benchmark guidelines.

The goal was to:
- Reduce attack surface  
- Enforce least privilege  
- Secure administrative access  
- Enable monitoring and threat detection  

---

# 🛠️ 1. Configuration Security

## 🔑 Admin Password & HTTPS Enforcement
- Changed default **admin credentials**
- Enabled **HTTPS-only WebGUI access**
- Disabled insecure HTTP access

## 🌐 System Configuration (Hostname, DNS)
- Configured:
  - Hostname  
  - Domain  
  - DNS servers  

## ⚠️ Security Banner & MOTD
- Configured **SSH warning banner**
- Set **Message of the Day (MOTD)**:


---

# 🔐 2. Access Control

## 🚫 Restrict WebGUI Access
- Restricted WebGUI access to **Management Network only**
- Blocked access from:
- WAN  
- User VLANs  

## 👤 User & Authentication Management
- Disabled or secured default accounts  
- Configured **role-based access control (RBAC)**  

## 🔑 Centralized Authentication (LDAP/RADIUS)
- Integrated **LDAP / RADIUS authentication**
- Enabled centralized authentication management  

## ⏱️ Session Timeout Configuration
- Configured session timeout ≤ **10 minutes**
- Enabled automatic logout for inactive sessions  

---

# 🌐 3. Firewall Rules & Segmentation

## 📜 Default Deny Strategy
- Implemented **deny-all by default policy**
- Allowed only explicitly required traffic  

## 🧱 Network Segmentation
Segmented the network into:
- LAN (User network)  
- DMZ (Public-facing services)  
- Management VLAN  
- Server VLAN  

## 🔄 Traffic Flow Rules

### Allowed Traffic
- Dev → DMZ (SSH)  
- Application → Database (Port 3306)  
- Finance → Application (Port 63455)  

### Denied Traffic
- Inter-VLAN traffic (by default)  
- Direct database access from user networks  

## ✅ Best Practices
- Used **aliases** for IPs and ports  
- Enabled logging on critical rules  
- Ordered rules from most specific → least specific  

---

# 🛡️ 4. Intrusion Detection & Prevention (Snort)

## 🚨 IDS/IPS Configuration
- Installed and enabled **Snort**
- Applied on:
- WAN interface  
- LAN interface (optional monitoring)  

## 📊 Rules & Detection
- Enabled:
- Emerging Threats rules  
- Community rule sets  
- Configured automatic rule updates  

## 🚫 Active Protection
- Enabled **IPS mode (blocking)**
- Automatically blocked malicious IP addresses  

---

# 📡 5. Logging & Monitoring

## 📜 Local Logging
Enabled logging for:
- Firewall events  
- Administrative logins  
- System changes  

## 🌍 Remote Logging (SIEM Integration)
- Configured **remote syslog forwarding**
- Integrated with **Splunk SIEM**  

## 📊 Logged Events
- Firewall traffic  
- VPN activity  
- IDS/IPS alerts  

---

# 🚀 Security Outcomes

- 🔒 Hardened firewall management access  
- 🚫 Reduced unauthorized access risk  
- 🧱 Implemented strong network segmentation  
- 🔍 Improved visibility with centralized logging  
- 🛡️ Enabled real-time threat detection and blocking  

---

# 🧪 Lab Environment

- **Firewall:** pfSense  
- **IDS/IPS:** Snort  
- **SIEM:** Splunk  
- **Architecture:** VLAN-based segmented network  

---

# ⭐ Key Takeaway

This project demonstrates **real-world firewall hardening aligned with CIS Benchmark standards**, applying:
- Defense-in-depth strategy  
- Network segmentation  
- Secure access control  
- Continuous monitoring  
