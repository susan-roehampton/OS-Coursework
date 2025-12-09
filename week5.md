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

```
sudo systemctl enable apparmor  
sudo systemctl start apparmor  
sudo aa-status  
```
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

```
sudo apt update  
sudo apt install unattended-upgrades -y  
sudo dpkg-reconfigure unattended-upgrades  
```

### Verification

sudo systemctl status unattended-upgrades  

### Purpose

- Ensures continuous security patching  
- Reduces risk of exploitation due to outdated packages  
- Maintains system integrity automatically  

### Evidence

Evidence 1: Installation
<img width="1273" height="916" alt="week5-unattended-install" src="https://github.com/user-attachments/assets/6da2ecac-16b1-4967-9156-1192ab5d5112" />

Evidence 2: Enable Screen
<img width="1278" height="914" alt="week5-unattended-enable" src="https://github.com/user-attachments/assets/64439658-f4a6-4045-be75-9875ab8faad3" />

Evidence 3: Verification
<img width="1274" height="918" alt="week5-unattended-verify" src="https://github.com/user-attachments/assets/abe7d04c-351e-405e-92c3-c04d8882f481" />


---

## 4. Fail2Ban – Intrusion Detection and Prevention

Fail2Ban is an intrusion detection system that protects the server against brute-force login attempts by banning malicious IP addresses.

### Installation and Activation

```
sudo apt install fail2ban -y  
sudo systemctl enable fail2ban  
sudo systemctl start fail2ban  
```

### Verification

sudo fail2ban-client status  
sudo fail2ban-client status sshd  

### Purpose

- Protects SSH against brute-force attacks  
- Automatically blocks malicious IPs  
- Improves overall network security  

### Evidence

Evidence 1 – Installation:
<img width="935" height="967" alt="week5-fail2ban-install" src="https://github.com/user-attachments/assets/d635da3d-ca4a-48f4-a218-6c009144229a" />

Evidence 2 – Service Started:
<img width="939" height="962" alt="week5-fail2ban-start" src="https://github.com/user-attachments/assets/ae7697dc-919d-4dfc-b721-1cc925dab724" />

Evidence 3 – Service Status:
<img width="938" height="935" alt="week5-fail2ban-status" src="https://github.com/user-attachments/assets/b7f6f844-cb2c-4f94-8c6c-e52a35beb3c5" />

Evidence 4 – SSH Jail Active:
<img width="950" height="972" alt="week5-fail2ban-sshd" src="https://github.com/user-attachments/assets/5b6f3101-8c73-40bd-b831-c6e793c67416" />


---

## 5. Security Baseline Verification Script (security-baseline.sh)

The security baseline verification script was created to automatically validate that all security configurations from Week 4 and Week 5 remain enforced.

#!/bin/bash
#!Security Baseline Verification Script
#!Verifies firewall, SSH, AppArmor, updates & Fail2Ban

echo "======== SECURITY BASELINE CHECK ========"

echo ""
echo "1. SSH SERVICE STATUS:"
systemctl status ssh --no-pager | head -n 5

echo ""
echo "2. FIREWALL STATUS (UFW):"
sudo ufw status verbose

echo ""
echo "3. FAIL2BAN STATUS:"
sudo systemctl status fail2ban --no-pager | head -n 5

echo ""
echo "4. FAIL2BAN SSH JAIL STATUS:"
sudo fail2ban-client status sshd

echo ""
echo "5. APPARMOR STATUS:"
sudo aa-status

echo ""
echo "6. AUTOMATIC UPDATES STATUS:"
cat /etc/apt/apt.conf.d/20auto-upgrades

echo ""
echo "7. ACTIVE USERS:"
cut -d: -f1 /etc/passwd

echo ""
echo "8. LISTENING NETWORK PORTS:"
ss -tuln

echo ""
echo "======== VERIFICATION COMPLETE ========"


### Script Execution

bash security-baseline.sh  

### Checks Performed by the Script

- SSH service status verification  
- UFW firewall status verification  
- AppArmor status verification  
- Fail2Ban status verification  
- Automatic update service verification  

This script ensures continuous compliance with the defined server security baseline.

### Evidence – Script Output:
<img width="1322" height="1017" alt="week5 -security-baseline-output" src="https://github.com/user-attachments/assets/5bc011ed-0abd-4b9d-be8d-eb61f6a5f21e" />

---

## 6. Remote Monitoring Script (monitor-server.sh)

A remote monitoring script was created on the Windows workstation using Git Bash. The script connects to the Ubuntu Server via SSH and collects real-time performance and security metrics.

#!/bin/bash
#!Remote Monitoring Script
#!Connects to server via SSH and collects performance metrics


SERVER_USER="susanserver"
SERVER_IP="192.168.56.103"

echo "===================================="
echo " Remote Server Monitoring Script"
echo "===================================="


echo "===== SERVER UPTIME ====="
ssh "$SERVER_USER@$SERVER_IP" "uptime"
echo""

echo "===== CPU USAGE ====="
ssh "$SERVER_USER@$SERVER_IP" "top -bin | grep 'Cpu(s)'"
echo ""

echo "===== MEMORY USAGE ====="
ssh "$SERVER_USER@$SERVER_IP" "free -h"
echo ""

echo "===== DISK USAGE (ROOT FILESYSTEM) ====="
ssh "$SERVER_USER@$SERVER_IP" "df -h /"
echo ""

echo "===== ACTIVE USERS ====="
ssh "$SERVER_USER@$SERVER_IP" "who"
echo ""

echo "===== LISTENING NETWORK PORTS ====="
ssh "$SERVER_USER@$SERVER_IP" "ss -tuln"
echo ""

echo "===== FAIL2BAN STATUS ====="
ssh -t "$SERVER_USER@$SERVER_IP" "sudo fail2ban-client status sshd"
echo ""

echo "===== UFW FIREWALL STATUS ====="
ssh -t "$SERVER_USER@$SERVER_IP" "sudo ufw status verbose"
echo ""

echo "===== APPARMOR STATUS ====="
ssh -t "$SERVER_USER@$SERVER_IP" "sudo aa-status"
echo ""

echo "===== MONITORING COMPLETE ====="

EOF


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
<img width="1303" height="585" alt="monitor-server-output1" src="https://github.com/user-attachments/assets/a4e82504-be1f-4479-a329-8b23c8ee44ec" />

<img width="1421" height="601" alt="monitor-server-output2" src="https://github.com/user-attachments/assets/865d3ea0-1502-421c-91b3-3cc506025a67" />

<img width="606" height="1089" alt="monitor-server-output3" src="https://github.com/user-attachments/assets/e28bb20e-575e-49f1-80e8-fba827440571" />

---

## 7. Critical Analysis

The implementation of AppArmor, Fail2Ban, and automatic security updates significantly strengthened the security posture of the Ubuntu Server. AppArmor provides mandatory access control at the application level, while Fail2Ban actively protects the system against brute-force SSH attacks. Automatic updates ensure that known vulnerabilities are patched without manual administrative effort.

The use of automated verification and monitoring scripts demonstrates how security configurations must be continuously validated rather than relying only on initial setup. While these security mechanisms introduce minimal performance overhead, the security benefits far outweigh the resource cost.

---

## 8. Reflection

During Week 5, I developed a strong understanding of layered security implementation and the importance of automation in modern system administration. I learned how intrusion detection, access control, automatic patching, and monitoring scripts work together to protect a Linux server. This week improved my confidence in writing shell scripts, using SSH for automation, and verifying security controls in a professional environment.

---

✅ Week 5 Completed Successfully

