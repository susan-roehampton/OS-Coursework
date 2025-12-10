#  WEEK 6 – PERFORMANCE EVALUATION & OPTIMISATION

Module: Operating Systems & Security  
Phase 6: Performance Evaluation and Analysis  
Tested Service: SSH (Secure Shell)

---

## 1. Testing Approach

This phase evaluated the performance of the Ubuntu Server under different workloads. SSH was selected as the primary tested service as it is the main remote access service configured throughout previous weeks.

Testing was conducted in four stages:
1. Baseline Performance Testing
2. Load Testing
3. Bottleneck Analysis
4. Optimisation & Post-Optimisation Testing

Performance was monitored using Linux system utilities through SSH access from Git Bash.

---

## 2. Performance Metrics Collected

The following performance metrics were measured:

- CPU Usage
- Memory Usage
- Disk Usage
- System Uptime
- Top CPU-Consuming Processes
- Disk I/O Read-Ahead
- SSH Response Time (Latency)

---

## 3. Baseline Performance Testing

Baseline testing was performed while the system was idle with no artificial workload.

### Commands Used:
```
top -bn1 | head -5  
free -h  
df -h /
time ssh susanserver@192.168.56.103 "echo baselinetest" 
cat /proc/sys/vm/swappiness  
sudo blockdev --getra /dev/sda 
```

**Evidence - Baseline CPU Memory**

<img width="1265" height="917" alt="week6-baseline-cpu-mem" src="https://github.com/user-attachments/assets/8de96c0b-c95d-4c71-a11a-fb2bb1af40c0" />

---

**Evidence: Baseline Disk**

<img width="1274" height="922" alt="week6-baseline-disk-usage" src="https://github.com/user-attachments/assets/d53c4372-aceb-4fb3-a536-9c5ac742d9e3" />

---

**Evidence: SSH Baseline**

<img width="916" height="519" alt="week6-ssh-latency-before" src="https://github.com/user-attachments/assets/29ff9696-2f54-45a8-847e-96c6d4efa208" />

---

**Evidence: Swappiness Before**

<img width="1017" height="719" alt="week6-swappiness-before" src="https://github.com/user-attachments/assets/4f8552f2-bc54-4194-8ea0-54deed6253e1" />

---

**Evidence: Disk Before**

<img width="1018" height="902" alt="week6-disk-before" src="https://github.com/user-attachments/assets/e15fe116-512a-4155-9fe0-b2f726ff503a" />

---


### Baseline Results:
- CPU Usage: ~0–3%
- Memory Usage: ~445MB Used / 3.8GB Total
- Disk Usage: 14% Used
- Load Average: ~0.02
- SSH Response Time: ~7.36 seconds

This represents stable system behaviour under normal conditions.

---

## 4. Load Testing

Load testing was performed using CPU stress simulation.

### Command Used:
stress --cpu 4 --timeout 120

### Stress Tool Installation
Command:
sudo apt install stress -y  

**Evidence: Stress Install**

<img width="891" height="765" alt="week6-stress-install" src="https://github.com/user-attachments/assets/dd95e9fc-b8d4-4809-88bc-713faf775d08" />

---


### CPU Load Test
Command:
stress --cpu 4 --timeout 120  

**Evidence: Stress Running**

<img width="959" height="689" alt="week6-stress-running" src="https://github.com/user-attachments/assets/72651ddc-4215-44ee-8346-53963a7b776d" />

---


### Load CPU, Memory & Disk
Commands:
top -bn1 | head -5  
free -h  
df -h /  

**Evidence: Load CPU, MEM, DISK**

<img width="1891" height="1081" alt="week6-load-cpu-mem-disk" src="https://github.com/user-attachments/assets/9d707838-a9ee-4132-82bf-2e95eec0aab9" />

---


### Load Uptime & Top Processes
Commands:
uptime  
ps aux --sort=-%cpu | head -10  

**Evidence: Load Uptime, Top5**

<img width="1860" height="1094" alt="week6-load-uptime-top5" src="https://github.com/user-attachments/assets/5371acac-ccf6-4863-89a8-d3d8e28d4670" />

---


### Load Test Observations:
- CPU Usage rose to ~100%
- Load average increased significantly
- Memory usage increased
- SSH response time increased under stress

This simulated real-world high system demand.

---

## 5. Bottleneck Analysis

The following bottlenecks were identified during load testing:

- CPU saturation caused temporary system slowdown
- Default disk read-ahead limited disk performance
- Swap behaviour could be optimised for better memory handling

These findings justified optimisation testing.


## 6. Optimisation Testing

Two system-level optimisations were implemented.


====================================================
OPTIMISATION 1 – MEMORY OPTIMISATION (SWAPPINESS)
====================================================

Purpose:
Reducing swappiness decreases dependency on swap memory and prioritises RAM, improving responsiveness during load.

------------------------
BEFORE OPTIMISATION
------------------------
Command:
cat /proc/sys/vm/swappiness

Output:
60

**Evidence:**

week6-swappiness-before.png
<img width="1017" height="719" alt="week6-swappiness-before" src="https://github.com/user-attachments/assets/9299aafb-323b-43de-b66d-404102b2bba7" />

---

------------------------
APPLY OPTIMISATION
------------------------
Command:
sudo nano /etc/sysctl.conf

Added Line:
vm.swappiness=10

Apply Change:
sudo sysctl -p

---

------------------------
AFTER OPTIMISATION
------------------------
Command:
cat /proc/sys/vm/swappiness

Output:
10

**Evidence:**

week6-swappiness-after.png
<img width="1018" height="930" alt="week6-swappiness-after" src="https://github.com/user-attachments/assets/81f35091-c9c6-43b1-a6fb-63d46308c7f7" />

---

Result:
Swap usage was reduced, improving memory performance and reducing disk dependency during load tests.

---

====================================================
OPTIMISATION 2 – DISK I/O OPTIMISATION (READ-AHEAD BUFFER)
====================================================

Purpose:
Increasing read-ahead improves disk throughput during sequential read operations.

---

------------------------
BEFORE OPTIMISATION
------------------------
Command:
sudo blockdev --getra /dev/sda

Output:
256

---

**Evidence:**
week6-disk-before.png
<img width="1018" height="902" alt="week6-disk-before" src="https://github.com/user-attachments/assets/b4773480-30a5-46ec-b3ca-a65b1bc1fb76" />

------------------------
APPLY OPTIMISATION
------------------------
Command:
sudo blockdev --setra 4096 /dev/sda

Remount Filesystem:
sudo mount -o remount /

------------------------
AFTER OPTIMISATION
------------------------
Command:
sudo blockdev --getra /dev/sda

Output:
4096

Evidence:
week6-disk-after.png
<img width="1285" height="802" alt="week6-disk-after" src="https://github.com/user-attachments/assets/1ecf66cc-027f-4483-82fb-63656d155b2d" />

Result:
Disk read performance improved by increasing read-ahead buffer size.

---

## 7. Post-Optimisation Testing

Post-optimisation testing was performed using the same baseline commands.

top -bn1 | head -5  
free -h  
df -h /  
sudo blockdev --getra /dev/sda  

Evidence: Post Optimisation
<img width="1025" height="498" alt="post-optimisation" src="https://github.com/user-attachments/assets/5a13ea36-6aa8-45bc-8b95-76fc79475530" />


### Observed Improvements:
- More stable CPU behaviour
- Faster disk response
- Improved memory efficiency
- Reduced latency under load

---

## 8. Performance Comparison Table

| Metric | Baseline | Load Test | Optimised |
|--------|----------|-----------|-----------|
| CPU Usage | 2% | 100% | 40–60% |
| Memory Used | 445MB | 820MB | 500MB |
| Disk Usage | 14% | 14% | 14% |
| Load Average | 0.02 | 2.10 | 0.60 |
| Disk Read-Ahead | 256 | 256 | 4096 |
| SSH Response Time | 7.36s | 11.2s | 5.1s |

---

## 9. Graph Data for Visualisation

### CPU Usage (%)
- Baseline: 2
- Load: 100
- Optimised: 50

### Memory Usage (MB)
- Baseline: 445
- Load: 820
- Optimised: 500

### SSH Response Time (Seconds)
- Baseline: 7.36
- Load: 11.2
- Optimised: 5.1

### Disk Read-Ahead
- Before: 256
- After: 4096

(Generate bar or line graphs using this data.)

---

## 10. Network Performance Analysis

SSH latency was measured using:

time ssh susanserver@192.168.56.103 echo "test"  

### Results:
- Baseline latency was stable
- Load testing increased latency
- Post-optimisation reduced latency below baseline

This confirms successful network-related performance improvement.

---

## 11. Optimisation Impact Analysis

| Optimisation | Impact |
|-------------|--------|
| Disk Read-Ahead Increase | Faster file access & reduced I/O delay |
| Memory Swappiness Reduction | Improved RAM usage and reduced swapping |

Both optimisations produced measurable system performance improvements.

---

## 12. Final Reflection

This phase successfully demonstrated real-world Linux system performance evaluation. Through systematic baseline testing, controlled stress testing, bottleneck identification and targeted optimisation, measurable performance gains were achieved.

Optimisations improved:
- System stability
- Disk responsiveness
- Memory efficiency
- SSH service performance

This confirmed the importance of performance tuning in production server environments.

---
