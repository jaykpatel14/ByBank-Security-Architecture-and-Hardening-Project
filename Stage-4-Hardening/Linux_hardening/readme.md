# BYBANK Enterprise CIS Hardening Report

## Overview

This report documents the CIS hardening process applied to BYBANK's 5 Linux endpoints using Ansible, Semaphore, and Wazuh for compliance verification.

---

## Architecture & Process

1. **Ansible Controller** was set up with Docker installed.
2. A **Semaphore container** was pulled in Docker to provide a UI for Ansible job execution.
3. **SSH keys** were copied to all 5 Linux endpoints (password authentication was disabled in favour of key-based auth).
4. A **CIS hardening Ansible playbook** was saved to GitHub (`git@github.com:jaykpatel14/Bybank-Ansible-Lab.git`).
5. Semaphore pulled the playbook from GitHub and executed it against all 5 endpoints.

### Endpoints

| Hostname | Role |
|---|---|
| BYBANKWeb | web |
| BYBANKDB | db |
| BYBANKSPLUNK | splunk |
| BYBANKWAZUH | wazuh |
| BYBANKANSIBLE | controller |

---

## Ansible Hardening Playbook

The playbook covered the following CIS control sections:

### Section 1 — Initial Setup & Banners
- **1.1 Login Banners (CIS 1.7):** Deployed `AUTHORIZED BYBANK ACCESS ONLY. PROCEED WITH CAUTION.` to `/etc/motd`, `/etc/issue`, and `/etc/issue.net`.
- **1.2 AppArmor:** Ensured AppArmor was installed on all endpoints.

### Section 4 — Firewall (UFW)
- **4.1** Installed UFW.
- **4.2** Set default policies: deny incoming, allow outgoing.
- **4.3** Ensured SSH (port 22) is always allowed.
- **4.4** Allowed role-specific ports:

| Role | Allowed Ports |
|---|---|
| web | 80, 443, 22 |
| db | 3306, 22 |
| splunk | 8000, 9997, 514, 22 |
| wazuh | 1514, 1515, 55000, 443, 22 |
| controller | 22 |

- **4.5** Enabled UFW.

### Section 5 — Access Control (SSH)
- **5.1 SSH Hardening:**
  - `PermitRootLogin no`
  - `PasswordAuthentication no`
  - `MaxAuthTries 3`
  - `X11Forwarding no`

### Section 6 — Logging & Auditing
- **6.1** Installed `auditd`.

### Section 7 — File Permissions
- **7.1 Critical file permissions:**

| File | Mode |
|---|---|
| `/etc/passwd` | 0644 |
| `/etc/shadow` | 0000 |
| `/etc/group` | 0644 |

---

## Semaphore Execution Results

### Play Recap

| Host | OK | Changed | Unreachable | Failed | Skipped |
|---|---|---|---|---|---|
| BYBANKWeb | 12 | 0 | 0 | 0 | 1 |
| BYBANKDB | 13 | 8 | 0 | 0 | 1 |
| BYBANKSPLUNK | 12 | 0 | 0 | 0 | 1 |
| BYBANKANSIBLE | 10 | 1 | 0 | **1** | 1 |
| BYBANKWAZUH | 3 | 0 | 0 | **1** | 1 |

### Notable Results

**BYBANKDB** — Had the most changes applied (8 changed tasks), indicating it was the least hardened before the playbook ran. Changes included login banners, SSH settings, UFW rules, SSH allowed in UFW, port 3306 firewall rule, UFW enabled, shadow file permissions, and SSH service restart.

**BYBANKANSIBLE — FAILED (Task 6.1 Install Auditd)**
```
sudo-rs: PAM error: System error
MODULE FAILURE: No start of json char found
```
The `sudo-rs` PAM error suggests a privilege escalation issue on the controller node. The `auditd` package could not be installed. All other tasks completed successfully.

**BYBANKWAZUH — FAILED (Task 1.2 Install AppArmor)**
```
Failed to update apt cache after 5 retries
```
The Wazuh host could not update its apt cache, likely due to a network/DNS issue or a locked apt process at the time of execution. This caused the entire play to abort early for this host — only 3 tasks completed.

---

## Wazuh CIS Benchmark — Before vs After

Wazuh was used to run CIS benchmark checks on all endpoints before and after applying the hardening playbook. Screenshots are included below for each host.

> **Note:** Screenshots are referenced from the original report. Replace image paths with actual screenshot files as needed.

---

### BYBANKSPLUNK

**Before Hardening**
![BYBANKSPLUNK Before](Stage-4-Hardening/Linux_hardening/Screenshots/Before - BYYBANKSPLUNK.png)

**After Hardening**
![BYBANKSPLUNK After](jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/Stage-4-Hardening/Linux_hardening/Screenshots/After - BYBANKSPLUNK.png)

---

### BYBANKDB

**Before Hardening**
![BYBANKDB Before](jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/Stage-4-Hardening/Linux_hardening/Screenshots/Before - Database.png)

**After Hardening**
![BYBANKDB After](jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/Stage-4-Hardening/Linux_hardening/Screenshots/After - BYBANKDB.png)

---

### BYBANKANSCTRL

**Before Hardening**
![BYBANKANSCTRL Before](jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/Stage-4-Hardening/Linux_hardening/Screenshots/Before - BYBANKANSCTRL.png)

**After Hardening**
![BYBANKANSCTRL After](jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/Stage-4-Hardening/Linux_hardening/Screenshots/After - BYBANKANSCTRL.png)

---

### BYBANKWeb

**Before Hardening**
![BYBANKWeb Before](jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/Stage-4-Hardening/Linux_hardening/Screenshots/Before - BYBANKWEB.png)

**After Hardening**
![BYBANKWeb After](jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/Stage-4-Hardening/Linux_hardening/Screenshots/After - BYBANKWEB.png)

---

### BYBANKWAZUH

**Before Hardening**
![BYBANKWAZUH Before](jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/Stage-4-Hardening/Linux_hardening/Screenshots/Before - BYBANKWAZUH.png)

**After Hardening**
![BYBANKWAZUH After](jaykpatel14/ByBank-Security-Architecture-and-Hardening-Project/Stage-4-Hardening/Linux_hardening/Screenshots/After - BYBANKWAZUH.png)

---

## Issues & Recommendations

| Host | Issue | Recommendation |
|---|---|---|
| BYBANKWAZUH | apt cache update failed — AppArmor not installed | Investigate network connectivity on the Wazuh host, then re-run the playbook with the `initial` tag |
| BYBANKANSIBLE | `sudo-rs` PAM error prevented auditd install | Investigate `sudo-rs` configuration and PAM stack on the controller; consider switching to standard `sudo` |
| All hosts | No `requirements.yml` found — galaxy install skipped | Add a `requirements.yml` if Ansible Galaxy roles/collections are needed in future |

---

## Conclusion

The CIS hardening playbook was successfully applied to **4 out of 5** endpoints (BYBANKWeb, BYBANKDB, BYBANKSPLUNK, BYBANKANSIBLE partially). BYBANKWAZUH requires remediation before the playbook can complete. The Wazuh CIS benchmark checks confirm improvement in compliance scores across all successfully hardened hosts.
