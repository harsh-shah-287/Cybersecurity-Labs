# Linux Auditing and Endpoint Telemetry Lab

## Objective

To implement Linux kernel-level auditing using `auditd`, generate high-fidelity endpoint telemetry, analyze audit events, and translate observed behavior into actionable detection logic.

This lab focuses on understanding how real-world endpoint telemetry is collected and how it supports SOC operations, detection engineering, and threat hunting.

---

## Environment

- OS: Kali Linux (VMware)
- User Context: kali (UID 1000)
- Auditing Framework: Linux Audit (`auditd`)
- Tools Used:
  - auditd
  - auditctl
  - ausearch
  - aureport
  - journalctl
  - Linux CLI utilities
  - Git

---

## Lab Structure

- Linux-Auditing/
- ├── README.md
- ├── audit-rules.md
- ├── event-analysis.md
- └── detection-notes.md

---

## Audit Configuration

The Linux audit subsystem was enabled and configured to collect security-relevant events at the kernel level.

### Active Audit Rules

1. **Process Execution Monitoring**
   - Captures all process executions using the `execve` syscall.
   - Enables visibility into:
     - Executed binaries
     - User and privilege context
     - Parent-child process relationships
     - Command execution timing

2. **Sudo Configuration Monitoring**
   - Monitors access and modification attempts to `/etc/sudoers`.
   - Detects:
     - Privilege escalation persistence attempts
     - Unauthorized configuration changes

Detailed rule definitions are documented in:
audit-rules.md


---

## Telemetry Analysis

Audit events were queried and analyzed using `ausearch` to understand:

- Event structure and fields
- User vs effective user context
- Command execution attribution
- Privileged activity visibility
- File access monitoring behavior

Key findings from event inspection are documented in:
event-analysis.md


---

## Detection Engineering Outcomes

Based on observed telemetry, multiple detection use cases were identified, including:

- Suspicious binary execution (LOLBin abuse)
- Privileged command execution by non-root users
- Repeated or automated command execution
- Sudo configuration tampering
- Off-hours or abnormal execution patterns

Detection logic emphasizes:

- Behavioral signals over signatures
- Correlation across multiple audit events
- False-positive awareness
- Operational feasibility

Detection concepts and logic are documented in:
detection-notes.md


---

## Key Learnings

- Linux audit logs provide kernel-level, tamper-resistant telemetry.
- Audit events offer higher fidelity than standard system logs.
- Proper rule scoping is critical to balance visibility and noise.
- Detection engineering begins with understanding raw telemetry.
- Correlation significantly improves alert confidence.
- Documentation is essential for repeatable SOC operations.

---

## SOC and Purple Team Relevance

This lab aligns with real-world practices used in:

- SOC monitoring and investigation
- Detection engineering
- Endpoint detection and response (EDR)
- Threat hunting
- Purple Team operations

Audit telemetry serves as a foundational data source for SIEM and EDR platforms in enterprise environments.

---

## Evidence

- Audit Rules: `audit-rules.md`
- Event Analysis: `event-analysis.md`
- Detection Logic: `detection-notes.md`

