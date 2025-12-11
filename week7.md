# Week 7 – Security Audit and System Evaluation  

## Task 1: Lynis Security Audit (Before & After Remediation)

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

