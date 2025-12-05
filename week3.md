# Week 3 – Application Selection for Performance Testing

---

## 1. Introduction

The aim of this week was to select suitable applications that represent different workload types in order to evaluate the performance of the Ubuntu Server operating system. Each application was chosen to stress a specific system resource so that CPU, memory, disk, and network performance can be analysed in later testing.

---

## 2. Application Selection Matrix

| Workload Type       | Application | Reason for Selection                                     |
|---------------------|-------------|----------------------------------------------------------|
| CPU-intensive       | stress-ng   | To generate high CPU load and test processor performance |
| Memory-intensive    | stress-ng   | To simulate high RAM usage under controlled conditions   |
| Disk I/O-intensive  | fio         | To test disk read and write speed and performance        |
| Network-intensive   | iperf3      | To measure network bandwidth and throughput              |
| Server Application  | Apache2     | To simulate a real-world web server workload             |

These applications were selected because they are lightweight, reliable, widely used in industry, and suitable for controlled testing in a virtual machine environment.

---

## 3. Installation Documentation (via SSH)

All applications will be installed remotely on the Ubuntu Server using SSH from the Windows workstation.

stress-ng (CPU and Memoery Testing Tool)
sudo apt install stress-ng -y

Disk I/O Testing Tool
The dd command is a built-in Linux utility and does not require installation.

Network Performance Testing Tool)
sudo apt install iperf3 -y

Apache2 Web Server (Server Application)
sudo apt install apache2 -y

---

## 4. Expected Resource Profiles

stress-ng

Very high CPU usage during stress testing
High memory usage when RAM tests are executed
Minimal disk and network activity

dd

Very high disk read and write activity
Moderate CPU usage
No network usage

iperf3

Very high network bandwidth usage
Low CPU usage
Low memory usage

Apache2

Moderate CPU usage under client load
Moderate memory usage
Continuous network activity during web requests

---

# 5. Monitoring Strategy

The following tools will be used to monitor system performance during testing:

-  top and htop – CPU and memory monitoring
-  free -h – Memory usage
-  df -h – Disk usage
-  iostat – Disk I/O statistics
-  iperf3 – Network throughput testing
-  ping – Network latency testing
-  uptime – System load averages

---

# Performance data will be collected during:

Baseline testing
Application stress testing
Post-optimisation testing

All results will be recorded in structured performance tables and visualised using charts and graphs in Week 6.

---

# 6. Reflection

This week helped me understand how different applications stress different system resources. Using both built-in and external monitoring tools demonstrated how operating systems handle CPU, memory, disk, and network workloads differently.
This preparation ensures that the performance testing in Week 6 will be structured, repeatable, and supported with quantitative evidence.

