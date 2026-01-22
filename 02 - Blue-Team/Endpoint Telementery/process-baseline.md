# Endpoint Process Baseline – Kali Linux VM 

## Objective

To establish a baseline of normal system processes, resource utilization, and execution behavior on the Kali Linux virtual machine.  
This baseline supports anomaly detection, behavioral monitoring, and future endpoint detection engineering.

---

## Environment

- OS: Kali Linux (VMware)
- Runtime: Fresh boot (~10 minutes uptime)
- Logged-in User: kali
- Baseline Tools:
  - ps
  - top

---

## Baseline Data Collection

### Command 1
```bash
ps aux | head -20
```

**Purpose**:
- Identify core system and kernel processes.
- Observe user ownership and process state.

### Command 2
```bash
top -b -n 1 | head -20
```

**Purpose:**
- Observe real-time CPU and memory utilization.
- Identify active and idle processes.

---

## Observed System Processes

### Kernel and Core services
Common baseline kernel threads observed:

| Process | Description |
|---------|-------------| 
|**/sbin/init (systemd**)|System initialization and service manager|
|**kthreadd**|Kernel thread manager|
|**kworker/***|Kernel background workers|
|**rcu_preempt**|Kernel memory synchronization|
|**ksoftirqd**|Interrupt handling|
|**migration/***|CPU task migration|
|**cpuhp/***|CPU hotplug management|

**Characteristics:**
- Owned by root
- Very low CPU and memory usage
- Persistent across uptime
- Mostly idle or sleeping states

### User processes
| Process | User | Description |
| ------- | ---- | ----------- |
| top | kali | Interactive monitoring tool |
| systemd | root | Init process |
| shell and terminals | kali | User session |

**Characteristics:**
- Limited number of user-initiated processes
- Short execution duration
- Low memory footprint

---

## Resource Utilization Baseline

### CPU Utilization

From top:
- User CPU: ~4.2%
- System CPU: ~4.2%
- Idle CPU: ~91.7%
- Load Average: 1.11 / 0.83 / 0.51

**Interpretation:**
- System is mostly idle.
- No sustained CPU-intensive processes.
- Normal idle behavior for lab environment.
---
### Memory Utilization

From top:
- Total Memory: ~3.8 GB
- Used Memory: ~1.0 GB
- Free Memory: ~1.6 GB
- Buffer/Cache: ~1.3 GB
- Swap Usage: 0%

**Interpretation:**
- Healthy memory utilization.
- No memory pressure or swapping.
- Normal caching behavior.
---
### Process Count
- Total Processes: ~257
- Running: 1
- Sleeping: ~256
- Zombie: 0

**Interpretation:**
- Normal Linux process distribution.
- No runaway or orphaned processes.

---

## Normal Behavioral Characteristics
- Baseline endpoint behavior includes:
- Dominance of kernel worker threads.
- Minimal active user processes.
- Stable CPU and memory utilization.
- No long-running network utilities.
- No elevated privilege activity during idle state.
- No abnormal parent-child process chains.

---
## Baseline Trust Indicators

Indicators considered normal:
- root-owned kernel threads with zero RSS.
- systemd as PID 1.
- kworker threads in sleeping state.
- Minimal interactive commands.
- No persistent listening services initiated by user.

---
## Baseline Anomaly Indicators 

Potential deviations from baseline may include:
- Unexpected high CPU utilization.
- Long-running user processes (nc, python, bash loops).
- Privileged tools executing without user intent.
- Unknown binaries running under root.
- Network tools running persistently.
- Abnormal parent-child relationships.

These will be treated as suspicious during detection analysis.

---

## Analyst Notes
- Baseline captured shortly after VM boot.
- Baseline should be refreshed periodically.
- Baseline comparison enables anomaly detection.
- Endpoint telemetry improves investigation accuracy.