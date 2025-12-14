#  Week 5 – Advanced Security and Monitoring Infrastructure

---

## 1. Introduction
In Week 5 the goals were implementing of advanced security controls and development of automated monitoring and verification mechanisms for the Ubuntu Server. This was accomplished by means of Mandatory Access Control(MAC), automatic patch management, intrusion detection as well as validation using shell scripts in an automated fashion. These configurations were implemented from the command line interface and accessed over SSH remotly from the workstation. This week directly addresses:

LO3: Assessing threats and applying security mechanisms
LO4: Demonstrating command-line proficiency and automation

---

## 2. Mandatory Access Control – AppArmor

AppArmor is a Mandatory Access Control(MAC) system used to prevent program approach to system resources situated on predefined security profiles. It gives an external security layer beyond traditional file permissions.

### Commands Used

```
sudo systemctl enable apparmor  
sudo systemctl start apparmor  
sudo aa-status  
```
These commands were used to allow AppArmor at boot, start the service, and detect that security profiles are actively enforced on the system.

### Purpose

- Enforces application-level access restrictions  
- Limits damage from compromised processes  
- Strengthens system defence using mandatory access control  

### Evidence

images/apparmor-status.png
<img width="1287" height="919" alt="week5-apparmor-status" src="https://github.com/user-attachments/assets/3b09d3cf-bdd0-4523-87c1-34e6d5f8e4da" />



---

## 3. Automatic Security Updates

Automatic security updates confirms that critical patches are installed without requiring manual intervention. This reduces exposure to known vulnerabilities.

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

Fail2Ban is an intrusion detection system that secure the server against brute-force login attempts by banning random IP addresses.

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

The security baseline verification script was created to significantly validate that all security configurations from Week 4 and Week 5 remain enforced.

#!/bin/bash

#Security Baseline Verification Script

#Verifies firewall, SSH, AppArmor, updates & Fail2Ban

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

### Evidence:

<img width="1282" height="804" alt="5-security-baseline" src="https://github.com/user-attachments/assets/801679ee-291d-4890-bee0-d4889e21dd30" />



---

## 6. Remote Monitoring Script (monitor-server.sh)

A remote monitoring script was created on the Windows workstation using Git Bash. The script connects to the Ubuntu Server via SSH and collects real-time performance and security metrics.

#!/bin/bash

#Remote Monitoring Script

#Connects to server via SSH and collects performance metrics


SERVER_USER="susanserver"
SERVER_IP="192.168.56.103"

echo "===================================="
echo " Remote Server Monitoring Script"
echo "===================================="

#show server uptime

echo "===== SERVER UPTIME ====="
ssh "$SERVER_USER@$SERVER_IP" "uptime"
echo""

#show CPU usage

echo "===== CPU USAGE ====="
ssh "$SERVER_USER@$SERVER_IP" "top -bin | grep 'Cpu(s)'"
echo ""

#show memory usage

echo "===== MEMORY USAGE ====="
ssh "$SERVER_USER@$SERVER_IP" "free -h"
echo ""

#show disk usage

echo "===== DISK USAGE (ROOT FILESYSTEM) ====="
ssh "$SERVER_USER@$SERVER_IP" "df -h /"
echo ""

#show active users

echo "===== ACTIVE USERS ====="
ssh "$SERVER_USER@$SERVER_IP" "who"
echo ""

#show listening network ports

echo "===== LISTENING NETWORK PORTS ====="
ssh "$SERVER_USER@$SERVER_IP" "ss -tuln"
echo ""

#show fail2ban status

echo "===== FAIL2BAN STATUS ====="
ssh -t "$SERVER_USER@$SERVER_IP" "sudo fail2ban-client status sshd"
echo ""
#show UFW firewall status

echo "===== UFW FIREWALL STATUS ====="
ssh -t "$SERVER_USER@$SERVER_IP" "sudo ufw status verbose"
echo ""

#show apparmor status

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

The script shows the professional remote administration practices by connecting performance monitoring with live security record.

### Evidence

images/monitor-server-output.png
<img width="1871" height="1023" alt="monitor-server-output1" src="https://github.com/user-attachments/assets/d8937872-9c32-4466-aaba-5caefb9a8fbd" />


<img width="1421" height="601" alt="monitor-server-output2" src="https://github.com/user-attachments/assets/865d3ea0-1502-421c-91b3-3cc506025a67" />

<img width="606" height="1089" alt="monitor-server-output3" src="https://github.com/user-attachments/assets/e28bb20e-575e-49f1-80e8-fba827440571" />

---

## 7. Critical Analysis
AppArmor, Fail2Ban adaptation and auto security updates have taken security position of the Ubuntu Server to a new level. AppArmor implements MAC (Mandatory Access Control) at the application level, while Fail2Ban represents one of the ways to protect your system from bruteforce SSH attacks. This is achieved through automatic updates which see to it that known vulnerabilities are patched without manual administrative effort.

The following are the reasons why you should not think of security configurations as something that should be set and forgotten, but rather something that needs regular checks: It is only a precautionary measure so since it makes less impact compared to setting the configurations initially performance might be slightly affected by these security measures yet the wider benefits in terms of security completely dwarf resource expenditure.

---

## 8. Reflection
In the fifth week, I was able to understand more on how to implement layered security through the use of some automated tools and also what automation really means in systems administration today. It''s possible I will learn, for example, how ids, aclas, autimatic checks and monitor scripts converge to safeguard a linux server. Those wrote codes with greater confidence this week while employed ssh for automation and also checking security controls in professional setting.

---
