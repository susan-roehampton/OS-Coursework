# Week 1 – System Planning and Distribution Selection

---

## 1. Introduction

The aim of this week was to plan the operating system deployment, select a suitable Linux server distribution, and configure the basic virtual environment required for the coursework. A dual-system architecture was designed to reflect real-world professional practice where system administration is carried out remotely.

---

## 2. System Architecture Overview

This coursework uses a dual-system architecture consisting of:

- Workstation: Windows host machine used for SSH access, monitoring, and GitHub documentation.
- Server: Ubuntu Server 24.04 LTS running headless (command-line only) inside VirtualBox.

The workstation securely connects to the server through a private virtual network. All administration of the server is performed remotely from the workstation.

**System Architecture Diagram**

<img width="700" alt="week1-system-architecture drawio (1)" src="https://github.com/user-attachments/assets/cb5ad3f2-b3b4-4880-9ded-55061514ae0c" />



---

The architecture consists of:

[Windows Workstation] → (VirtualBox NAT Network) → [Ubuntu Server 24.04 LTS]

The Windows workstation acts as the control machine, while the Ubuntu Server acts as the target system for configuration, security hardening, monitoring, and performance testing.

---

## 3. Distribution Selection Justification

Ubuntu Server 24.04 LTS was selected as the server operating system for the following reasons:

- Long-Term Support (LTS) provides extended security updates and stability.
- Excellent documentation and a very large user community.
- Widely adopted in industry and cloud environments.
- Strong support for security tools such as UFW, Fail2Ban, AppArmor, and Lynis.

### Alternative Distributions Considered:
- **Debian Server:** Very stable but slower update cycle.
- **CentOS Stream:** Good enterprise features but more complex for beginners.

Ubuntu Server was chosen because it provides a balance between professional-grade stability and beginner-friendly documentation.

---

## 4. Workstation Configuration Decision

The Windows host machine was selected as the workstation. This allows the use of:

- Windows PowerShell for SSH access.
- A web browser for GitHub Pages documentation.
- Monitoring and analysis tools directly on the host.

This approach reflects real-world industry practice where administrators often manage Linux servers remotely from Windows machines.

---

## 5. Network Configuration

The VirtualBox network is configured using **NAT (Network Address Translation)**.

### Reasons for choosing NAT:
- Provides automatic IP addressing.
- Ensures secure internal communication.
- Keeps the server isolated from external public networks.
- Allows outbound internet access for updates.

This configuration meets the security and isolation requirements of the coursework.

---

## 6. System Specifications (CLI Evidence)

The following commands were executed on the Ubuntu Server to document system specifications:

-  uname -a  – Displays kernel and system information  
-  free -h  – Displays memory usage  
-  df -h  – Displays disk usage  
-  ip addr  – Displays network configuration and IP address  
-  lsb_release -a  – Displays Ubuntu version information  

Screenshot evidence of these commands is provided below:
<img width="1284" height="835" alt="Screenshot 2025-12-05 030404" src="https://github.com/user-attachments/assets/732092e2-31ed-40d8-89a2-6edb3a59a068" />


---

## 7. Reflection

This week focused on system planning, operating system selection, workstation design, and initial Ubuntu Server deployment. I learned how to install a headless Linux server, configure virtual networking, and use fundamental Linux commands to review system hardware and software information. This week provided a strong technical foundation for the security configuration and performance analysis that will be implemented in later weeks.
