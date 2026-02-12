# Audit Event Analysis – Linux Endpoint Telemetry

## Objective

To analyze Linux audit events generated from execution monitoring and file watch rules in order to understand event structure, field visibility, and detection-relevant attributes.

This analysis forms the foundation for reliable detection engineering.

---

## Data Sources

- Audit daemon (`auditd`)
- Query tool: `ausearch`
- Active rules:
  - execve syscall monitoring
  - sudoers file watch

---

## Commands Used for Analysis

```bash
sudo ausearch -k exec_monitor
sudo ausearch -k exec_monitor -i
sudo ausearch -k sudo_watch
```
---
## Sample Event Structure
Observed audit events contain the following major fields:

| Field | Description |
| ----- | ----------- |
|timestamp | Exact execution time |
|exe | Executed binary path |
|comm | Command name |
|uid | Real user |
|euid | Effective user |
|pid | Process ID |
|ppid | Parent process ID |
|tty | Terminal |
|cwd | Working directory |
|success | Execution status |
|syscall | System call number |
|key | Audit rule identifier |

---

## Execution Telemetry Observations 
Monitored executions captured: 
- Standard user commands (ls, whoami, id)
- Privileged commands (sudo ls /root)
- Network utility execution (curl)
- Shell execution (bash)

Each execution produced structured records including:
- Exact binary path
- User and privilege context
- Parent process relationship
- Timestamp accuracy
- Terminal origin

---

## Privilege Monitoring Observations
File watch rule detected access events on: 
```bash 
/etc/sudoers
```
Observed event types:
- File open attempts
- Permission checks
- Metadata access

These events validate audit file monitoring capabilities.

---

## Telemetry Quality Assessment
Strengths:
- High fidelity process execution visibility
- Precise timestamps
- Strong attribution
- Kernel-level capture
- Tamper resistance

Limitations:
- High volume potential
- Requires tuning
- Not application aware
- Requires parsing normalization

---

## Detection-Relevant Fields
Fields most useful for detection logic:
- exe
- comm
- uid / euid
- tty
- cwd
- success
- key
- ppid

These enable behavioral correlation and filtering.

---

## Analyst Notes
- Audit logs provide superior visibility compared to journal logs.
- Event normalization is critical for SIEM ingestion.
- Rule tuning reduces noise.
- Field consistency supports automation.