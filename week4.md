# Week 4 – Initial System Configuration & Security Implementation

---

## 1. Introduction

This week focused on deploying the Ubuntu Server securely and implementing foundational security controls. All configurations were performed remotely via SSH from the workstation,
in line with the administrative constraints of the coursework.

---

## 2. User Privilege Management

A non-root administrative user named `studentadmin` was created to enforce the principle of least privilege and reduce the security risks associated with direct root usage.

### 📸 Evidence – Non-root Administrative User
![Non-root user creation](images/week4-nonroot-user.png)


### Commands Used:
sudo adduser studentadmin  
sudo usermod -aG sudo studentadmin  

The user was switched successfully using:
su - studentadmin

This confirms the user had been created correctly and granted administrative privileges.

---

## 3. Firewall Configuration

The Uncomplicated Firewall (UFW) was enabled and configured to allow SSH access while blocking all other incoming connections.

### 📸 Evidence – Firewall Enabled
![UFW Enabled](images/week4-ufw-enable.png)

### 📸 Evidence – Firewall Rules
![UFW Status](images/week4-ufw-status.png)


### Commands Used:
sudo ufw allow ssh  
sudo ufw enable  
sudo ufw status verbose  

Firewall Status Confirmed:
- Firewall is active and enabled on system startup
- Default policy: Deny incoming, Allow outgoing
- SSH (port 22) is explicitly allowed

---

## 4. SSH Hardening

The SSH configuration file was edited to disable root login for enhanced security.

### 📸 Evidence – Root Login Disabled
![SSH Root Login Disabled](images/week4-ssh-after.png)


### File Modified:
/etc/ssh/sshd_config  

### Configuration Change:
PermitRootLogin no  

The SSH service was restarted to apply changes:
sudo systemctl restart ssh  

This ensures that direct root access via SSH is now blocked.

---

## 5. Remote Administration Verification

A successful SSH connection was verified after security hardening to confirm that remote administration remained functional.

ssh susanserver@10.0.2.15  

The login completed successfully using the non-root administrative user.

### 📸 Evidence – Successful SSH Login After Hardening
![SSH Login Success](images/week4-ssh-login-after-hardening.png)


---

## 6. Reflection

This week provided hands-on experience of applying real-world Linux server security practices. Creating a non-root user significantly reduced the attack surface of the system. Configuring and enabling the firewall strengthened network protection. 
Disabling root SSH access further improved security against brute-force attacks. Successfully reconnecting after all changes confirmed that secure remote administration was still reliable and functional.

---
