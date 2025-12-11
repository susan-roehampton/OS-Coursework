# Week 7 – Security Audit and System Evaluation  

## [Task 1: Lynis Security Audit (Before & After Remediation)]

### 1. Introduction
This phase focuses on evaluating the security posture of the Ubuntu server using **Lynis**, a recognised auditing tool for UNIX-based systems. Lynis performs an extensive assessment of system configuration, hardening, services, authentication mechanisms, and known security gaps. The objective of this task was to run a baseline audit, apply recommended security improvements, and then perform a second audit to measure improvement.

---

## 2. Installing Lynis
Lynis was installed from the official Ubuntu repository:

```bash
sudo apt install lynis -y
```

After installation, the system confirmed that Lynis and its associated components were successfully configured.

**Evidence**

<img width="1283" height="805" alt="week7-lynis-install" src="https://github.com/user-attachments/assets/92c3eb64-0b2b-4fc2-b70d-b0d25948f031" />


---

## 3. Baseline Lynis Scan (Before Remediation)

The initial system audit was performed:

```bash
sudo lynis audit system
```


**Evidence**

<img width="1278" height="802" alt="week7-lynis-before" src="https://github.com/user-attachments/assets/0e320bae-281d-442a-b34c-b0ae89e3d865" />

---

### Baseline Hardening Index
- **Hardening Index:** 60  
- **Tests Performed:** 256  
- **Plugins Enabled:** 1  

### Key Baseline Findings

- The firewall was enabled and functioning correctly.  
- No malware scanner was installed on the system.  
- Compliance checks were incomplete or partially configured.  
- SSH configuration required improvement to meet best-practice security standards.  
- Several security audit modules reported missing or weak configurations.

---

### Baseline Recommendations
Lynis highlighted several important improvements:
- Install at least one **malware scanner**
- Harden SSH configuration
- Enable automatic security updates
- Improve audit logging and system integrity checks

---

## 4. Security Improvements Applied

### 4.1 Firewall Verification
The firewall was confirmed to be active and configured:

```bash
sudo ufw enable
sudo ufw allow OpenSSH
```

**Evidence**

<img width="1281" height="809" alt="wee7-ufw" src="https://github.com/user-attachments/assets/434b5961-2e8c-48d9-82ce-9f67acde4243" />

---

### 4.2 Installing Malware Scanner

```bash
sudo apt install rkhunter -y
sudo rkhunter --update
```

**Evidence**

<img width="1291" height="809" alt="week7-rkhunter-install" src="https://github.com/user-attachments/assets/f599d07e-dd21-421c-943e-b04f4a57e948" />


This addressed the previously missing malware detection capability.

---

### 4.3 Enabling Automatic Security Updates

The system prompted configuration of unattended upgrades.  
**“Yes”** was selected to automatically apply important updates, reducing long-term exposure to vulnerabilities.

**Evidence**

<img width="1279" height="800" alt="week7-auto-updates" src="https://github.com/user-attachments/assets/1e155e48-f6f1-4749-9edb-ef38731c343c" />

---

### 4.4 SSH Hardening

The SSH configuration file (/etc/ssh/sshd_config) was reviewed and adjusted according to Lynis suggestions, including:

- Reviewing authentication options  
- Ensuring unnecessary features remained disabled  
- Confirming logging and client restrictions  

SSH was restarted to apply changes:

```bash
sudo systemctl restart ssh
```

**Evidence**

<img width="1285" height="807" alt="week7-ssh-hardening" src="https://github.com/user-attachments/assets/abf1bb2d-c109-462f-b23b-fba5a1cf69d7" />

---

## 5. Lynis Second Audit (After Remediation)

A follow-up audit was performed after system hardening:

```bash
sudo lynis audit system
```

**Evidence**

<img width="1287" height="810" alt="week7-lynis-after" src="https://github.com/user-attachments/assets/a4ccc3ef-7efc-4d71-91e2-86da98d9574a" />

---

### Updated Hardening Index
- **Hardening Index:** 61  
- **Tests Performed:** 261  
- **Plugins Enabled:** 1  

### Improvements Observed After Remediation

- A malware scanner (rkhunter) was installed successfully.  
- SSH configuration was improved to remove weak defaults.  
- Automatic security updates were enabled to reduce vulnerability exposure.  
- More Lynis security checks were completed during the second audit.  
- Several prior warnings were resolved, reducing the system’s risk level.

Although the score increased modestly (60 → 61), it reflects incremental compliance with recommended hardening controls, as Lynis becomes stricter as more enhancements are applied.

---

## 6. Remaining Risks and Future Hardening Steps
Lynis reported a few additional areas for long-term improvement:

- Implement stronger compliance modules  
- Enhance system logging policies  
- Consider integrity-monitoring utilities like AIDE  
- Remove or disable unnecessary system services  

These areas will be addressed in later tasks within Week 7.

---

## 7. Conclusion
This task successfully completed the full Lynis auditing cycle:

1. Baseline assessment
2. Application of recommended security hardening steps
3. Post-remediation audit and improvement evaluation

The security posture of the server improved, demonstrated by the updated hardening index and corrected vulnerabilities. The system now aligns more closely with recognised security best practices.

---


## [Task 2:  Network Security Assessment]

## Installing Nmap (Host Machine)

Nmap was installed on the Windows host system using the following command:
```
winget install nmap
```

This successfully downloaded and installed the Nmap package required for external network scanning.

**Evidence**

<img width="1526" height="910" alt="week7-nmap-install" src="https://github.com/user-attachments/assets/eac777b8-c620-4c4e-92a7-b18a2475c497" />

---

## Nmap Scan

A full port and service scan was performed using Nmap from the host machine to assess exposed network services on the Ubuntu server.

Command used:
```
nmap -sV -Pn 192.168.56.103
```

**Purpose:**

- Identify open ports
- Detect service versions
- Check for unnecessary or vulnerable network services

## Nmap Scan Results

Only 1 open port detected:
 - Port 22/tcp — OpenSSH 9.6p1 (Ubuntu)

All other 999 ports are filtered, indicating the firewall is actively restricting access.

The host responded instantly (0.00s latency), meaning the system is reachable and stable.

**Evidence**

<img width="1529" height="916" alt="week7--nmap-scan" src="https://github.com/user-attachments/assets/3066aa55-b935-4873-92d3-451e90810ae4" />

---

## Interpretation

The system exposes only SSH, which is expected for remote administration.
The firewall (UFW) is configured correctly, reducing the attack surface significantly.
No unnecessary services (HTTP, FTP, SMB, database servers, etc.) are running — this is ideal for a hardened system.

##  Security Assessment Conclusion

The Nmap scan confirms that the server's network footprint is minimal and secure.
Only a single managed service (SSH) is exposed, and its version is up to date.
This aligns with best practices for system hardening.

---


## [Task 3: SSH Security Verification]

This section documents the verification of SSH configuration, service state, and version to ensure the server is securely configured and functioning correctly.

### 1. SSH Configuration Review

The SSH configuration file was inspected using:

```
sudo nano /etc/ssh/sshd_config
```

Key settings observed in the configuration:

- PermitRootLogin no
- PasswordAuthentication yes (required for coursework testing)
- X11Forwarding no
- Subsystem sftp /usr/lib/openssh/sftp-server

These settings confirm that root login is disabled, unnecessary forwarding features are turned off, and only required components are active.

---

### 2. SSH Service Status Check

The status of the SSH service was verified with:

```
sudo systemctl status ssh
```

**Verified Findings:**

- Service state: active (running)
- Startup state: enabled (starts automatically on boot)
- Listening on port 22
- No failures or repeated restarts reported

This confirms that the SSH server is stable and functioning properly.

---

### SSH Version Verification

The SSH client/server version was checked using:

```
ssh -V
```

**Result:**

- OpenSSH_9.6p1 Ubuntu 3ubuntu13.14
- OpenSSL 3.0.13

This confirms the system is running an up-to-date and secure version of OpenSSH.

---

**Evidence**

<img width="1289" height="798" alt="week7-ssh-verification" src="https://github.com/user-attachments/assets/734a46d8-325a-4fbb-8edc-ad61ca057909" />

---

### Summary of SSH Security State

Based on configuration, service status, and version inspection:

- SSH is correctly installed and running.
- Root login is disabled.
- Essential security defaults are enforced.
- No insecure or deprecated SSH options were found.
- The service is updated and maintained.

This verifies that SSH on the server meets the requirements for Week 7's security audit.

---


## [Task 4: Access Control Verification]

This section confirms that only authorised users have system access and that administrative permissions are correctly assigned.

---

### 1. User Identity
The `id` command was used to verify the active account.  
The user **susansserver** is correctly identified as the main system account and has expected access rights.

---

### 2. Group Membership
Group membership was checked using:

```bash
grep -E "sudo|adm" /etc/group
```

- The **sudo** group includes: susansserver and studentadmin  
- The **adm** group contains standard system monitoring users  

Both accounts with elevated rights are intentional and acceptable.

---

### 3. Home Directory Check
Home directories were reviewed to ensure each user’s data is private.

```bash
ls -l /home
```

Each user has their own home folder, with no cross-access between accounts.

---

### 4. Sudo Privilege Verification
Sudo capabilities were checked using:

```bash
sudo -l
```
**Result**
The account **susansserver** is allowed to run all commands with elevated privileges, which is appropriate for an administrator.

---

###  Summary
- User identities are correct  
- Only authorised accounts have admin access  
- Home directories are private  
- Sudo privileges are properly assigned  

System access controls are configured securely and meet required standards.

**Evidence**

<img width="1288" height="800" alt="week7-access-control" src="https://github.com/user-attachments/assets/27d2efa9-b7ae-4cd9-8240-84504c2a5156" />

---

## [Task 5 — Service Audit (Running Services & Justification)]

A service audit was conducted to identify active services and determine whether they are required for system operation and security.

Commands used:
```bash
systemctl list-units --type=service --state=running
systemctl --failed
```

**Evidence**

<img width="1280" height="807" alt="week7-service-audit" src="https://github.com/user-attachments/assets/e58b0b5f-ec99-41f9-aa91-293c3f74ef33" />

---

## 1. Summary of Findings
A total of **23 active services** were detected. Most are core components needed for Ubuntu to operate normally, while a few optional services are present but not harmful.

---

## 2. Analysis of Running Services

### 2.1 Essential System Services
- **cron.service** — Handles scheduled jobs.
- **systemd-journald.service** — Collects system logs.
- **rsyslog.service** — Additional logging service.
- **systemd-logind.service** — User session management.
- **systemd-udevd.service** — Hardware device handling.
- **user@1000.service** — User session manager.

### 2.2 Networking & Communication
- systemd-resolved.service
- dbus.service
- getty@tty1.service


### 2.3 Security & Update Services
- **ssh.service** — Secure remote login.
- **fail2ban.service** — Protects against brute-force attempts.
- **unattended-upgrades.service** — Automatic security updates.
- **fwupd.service** — Firmware update support.


### 2.4 User & System Functionality
- **polkit.service** — Authorization control.
- **systemd-timesyncd.service** — Time synchronization.
- **systemd-journald.service** — System event logging.

### 2.5 Optional / Non-Essential Services
- **ModemManager.service** — Not required in a VM.
- **multipathd.service** — Not needed (no multipath storage).
- **udisks2.service** — Desktop-oriented; optional.
- **upower.service** — Laptop power management; unnecessary here.
- **iperf3.service** — Only needed for testing (can be disabled afterward).

---

## 3. Failed Services Check
```bash
systemctl --failed
```
Result: 0 loaded units listed. (No services are failing)

**Evidence**

<img width="1287" height="808" alt="week7-service-failed" src="https://github.com/user-attachments/assets/c27bfb6f-2d4a-4c77-b02a-8f7fabded74b" />

---

## 4. Conclusion
The audit confirms that essential networking, logging, SSH, and update services are running correctly.  
Optional services are present but do not pose a security risk and can be disabled to further reduce surface area.  
Overall, the system is secure, stable, and operating as expected.


