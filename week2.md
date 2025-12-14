# Week 2 – Security Planning and Testing Methodology

---

## 1. Introduction

The goal of this week was to design a security baseline for the Ubuntu Server and to create a structured performance testing methodology. This planning stage is necessary to ensure that the server is protected against common threats while also allowing safe and reliable performance evaluation in later weeks.

---

## 2. Performance Testing Plan

All performance testing will be conducted remotely from the Windows workstation using SSH. Performance metrics will be collected before and after optimisation to allow direct comparison.

### Testing Approach
- Establish a baseline performance before any heavy workloads are applied.
- Apply controlled application workloads.
- Measure CPU, memory, disk, and network usage.
- Identify performance bottlenecks.
- Apply optimisations.
- Re-test and compare results.

### Monitoring Tools to be Used
- top – Real-time process monitoring
- htop – Enhanced CPU and memory monitoring
- free -h – Memory usage
- df -h – Disk usage
- iostat – Disk I/O statistics
- iperf3 – Network throughput testing
- ping – Network latency testing

All monitoring will be documented with screenshots and performance tables.

---

## 3. Security Configuration Checklist

The following security controls will be implemented in later weeks:

### SSH Hardening
- Disable root login via SSH
- Use key-based authentication
- Change default SSH configuration to improve security
- Limit SSH access to the workstation IP only

### Firewall Configuration
- Enable UFW firewall
- Allow SSH traffic only from the workstation
- Block all other unnecessary inbound traffic

### Mandatory Access Control
- Use AppArmor to enforce application-level security
- Ensure unwanted services are restricted

### Automatic Security Updates
- Enable unattended-upgrades
- Ensure security patches are installed automatically

### User Privilege Management
- Create a non-root administrative user
- Use sudo for privilege escalation
- Enforce least-privilege access

### Network Security
- Isolate server using a private NAT network
- Restrict exposed services
- Monitor network traffic

This checklist will be verified in Week 5 using an automated security baseline script.

---

## 4. Threat Model

Three major security threats were identified for this environment.

### Threat 1 – Brute-Force SSH Attacks
Description: Attackers may attempt to guess SSH passwords remotely.  
Impact: Unauthorized access to the server.  
Mitigation:
- Use SSH key-based authentication
- Disable password authentication
- Install and configure Fail2Ban
- Restrict SSH access using a firewall

---

### Threat 2 – Unauthorized Privilege Escalation
Description: A compromised user account could attempt to gain root privileges.  
Impact: Full system compromise.  
Mitigation:
- Enforce least-privilege access
- Use sudo instead of direct root login
- Monitor logs for suspicious activity
- Apply AppArmor profiles

---

### Threat 3 – Unpatched Vulnerabilities
Description: Outdated packages may contain known security vulnerabilities.  
Impact: Remote exploitation of known system flaws.  
Mitigation:
- Enable automatic system updates
- Regularly update packages
- Conduct security audits using Lynis

---

## 5. Reflection

This week focused on planning rather than implementation. I developed a clear understanding of how security controls work together to protect a Linux server. Designing the threat model helped me understand how attackers target systems and how defence mechanisms such as firewalls, SSH hardening, and automatic updates reduce risk. This planning stage will guide all security configurations in the upcoming weeks.
