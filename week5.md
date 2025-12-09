#  Week 5 – Advanced Security and Monitoring Infrastructure

---

## 1. Introduction

The purpose of Week 5 was to implement advanced security controls and develop automated monitoring and verification mechanisms for the Ubuntu Server. This phase focused on strengthening system security through Mandatory Access Control, automatic patch management, intrusion detection, and automated validation using shell scripts. All configurations were implemented via the command-line and verified remotely using SSH from the workstation.

This week directly addresses:
- LO3: Assessing threats and applying security mechanisms  
- LO4: Demonstrating command-line proficiency and automation  

---

## 2. Mandatory Access Control – AppArmor

AppArmor is a Mandatory Access Control (MAC) system used to restrict program access to system resources based on predefined security profiles. It provides an additional security layer beyond traditional file permissions.

### Commands Used

sudo systemctl enable apparmor  
sudo systemctl start apparmor  
sudo aa-status  

These commands were used to enable AppArmor at boot, start the service, and verify that security profiles are actively enforced on the system.

### Purpose

- Enforces application-level access restrictions  
- Limits damage from compromised processes  
- Strengthens system defence using mandatory access control  

### Evidence

images/apparmor-status.png
<img width="1287" height="919" alt="week5-apparmor-status" src="https://github.com/user-attachments/assets/3b09d3cf-bdd0-4523-87c1-34e6d5f8e4da" />



---

## 3. Automatic Security Updates

Automatic security updates ensure that critical patches are installed without requiring manual intervention. This reduces exposure to known vulnerabilities.

### Commands Used

sudo apt update  
sudo apt install unattended-upgrades -y  
sudo dpkg-reconfigure unattended-upgrades  

### Verification

sudo systemctl status unattended-upgrades  

### Purpose

- Ensures continuous security patching  
- Reduces risk of exploitation due to outdated packages  
- Maintains system integrity automatically  

### Evidence

images/auto-updates-status.png

---

## 4. Fail2Ban – Intrusion Detection and Prevention

Fail2Ban is an intrusion detection system that protects the server against brute-force login attempts by banning malicious IP addresses.

### Installation and Activation

sudo apt install fail2ban -y  
sudo systemctl enable fail2ban  
sudo systemctl start fail2ban  

### Verification

sudo fail2ban-client status  
sudo fail2ban-client status sshd  

### Purpose

- Protects SSH against brute-force attacks  
- Automatically blocks malicious IPs  
- Improves overall network security  

### Evidence

images/fail2ban-status.png
<img width="938" height="935" alt="week5-fail2ban-status" src="https://github.com/user-attachments/assets/ec0233ee-915c-4f40-8c50-8ecccdd4db5b" />

---

## 5. Security Baseline Verification Script (security-baseline.sh)

The security baseline verification script was created to automatically validate that all security configurations from Week 4 and Week 5 remain enforced.

### Script Execution

bash security-baseline.sh  

### Checks Performed by the Script

- SSH service status verification  
- UFW firewall status verification  
- AppArmor status verification  
- Fail2Ban status verification  
- Automatic update service verification  

This script ensures continuous compliance with the defined server security baseline.

### Evidence

images/security-baseline-script-output.png
<img width="1322" height="1017" alt="week5 -security-baseline-output" src="https://github.com/user-attachments/assets/15e88946-becf-4256-b9e1-af7fda83192f" />

---

## 6. Remote Monitoring Script (monitor-server.sh)

A remote monitoring script was created on the Windows workstation using Git Bash. The script connects to the Ubuntu Server via SSH and collects real-time performance and security metrics.

### Metrics Collected

- Server uptime  
- CPU usage  
- Memory usage  
- Disk usage  
- Active users  
- Listening network ports  
- Fail2Ban status  
- UFW firewall status  
- AppArmor status  

### Script Execution

./monitor-server.sh  

The script demonstrates professional remote administration practices by combining performance monitoring with live security verification.

### Evidence

images/monitor-server-output.png

---

## 7. Critical Analysis

The implementation of AppArmor, Fail2Ban, and automatic security updates significantly strengthened the security posture of the Ubuntu Server. AppArmor provides mandatory access control at the application level, while Fail2Ban actively protects the system against brute-force SSH attacks. Automatic updates ensure that known vulnerabilities are patched without manual administrative effort.

The use of automated verification and monitoring scripts demonstrates how security configurations must be continuously validated rather than relying only on initial setup. While these security mechanisms introduce minimal performance overhead, the security benefits far outweigh the resource cost.

---

## 8. Reflection

During Week 5, I developed a strong understanding of layered security implementation and the importance of automation in modern system administration. I learned how intrusion detection, access control, automatic patching, and monitoring scripts work together to protect a Linux server. This week improved my confidence in writing shell scripts, using SSH for automation, and verifying security controls in a professional environment.

---

✅ Week 5 Completed Successfully

