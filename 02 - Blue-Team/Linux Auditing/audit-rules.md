# Linux Audit Rules – Telemetry Configuration

## Objective

To document the audit rules deployed during the lab, explain their purpose, and define what security telemetry each rule generates.  
These rules establish visibility into command execution and privileged configuration changes.

---

## Audit Subsystem Overview

Audit rules are enforced at the kernel level and capture security-relevant events before user-space processes can modify or suppress them.

Rules are loaded using:

```bash
auditctl
```
Rules are identified using keys (-k) for efficient searching and correlation.

---

### Active Rules Summary
| Rule Key | Rule Type | Purpose |
| -------- | --------- | ------- |
|exec_monitor | Syscall rule | Monitor all process executions |
|sudo_watch	| File watch rule | Monitor access to sudo configuration |

---

## Rule 1 – Process Execution Monitoring
### Rule Definition
```bash
sudo auditctl -a always,exit -F arch=b64 -S execve -k exec_monitor 
```

### Rule Explanation
| Component	| Description |
| --------- | ----------- | 
|-a always,exit | Log event on syscall exit |
|-F arch=b64 | Monitor 64-bit system calls |
|-S execve | Capture process execution |
|-k exec_monitor | Assign searchable key |

### Telemetry Generated
This rule captures:
- Executed binary path
- Command name
- User and effective user
- Parent process ID
- Working directory
- Terminal session
- Execution success/failure
- Timestamp

### Security Value
Enables detection of:
- Unauthorized command execution
- LOLBin abuse
- Privilege escalation activity
- Suspicious parent-child process chains
- Malware execution attempts

---

## Rule 2 – Sudoers Configuration Monitoring
### Rule Definition
```bash
sudo auditctl -w /etc/sudoers -p wa -k sudo_watch
```
### Rule Explanation:

| Component | Description |
| --------- | ----------- | 
|-w	| Watch file |
|/etc/sudoers | Target file |
|-p wa | Monitor write and attribute changes |
|-k sudo_watch | Assign searchable key |

### Telemetry Generated
This rule captures:
- Access attempts to sudoers file
- Modification attempts
- Permission changes
- File metadata changes
- User attribution
- Timestamp

### Security Value
Enables detection of:
- Privilege persistence attempts
- Unauthorized privilege escalation
- Configuration tampering
- Insider threats

---

## Rule Validation
Verification command:
```
sudo auditctl -l
```
Expected output:
- exec_monitor rule visible
- sudo_watch rule visible

---

## Rule Persistence Considerations
Current rules are temporary and reset on reboot.

### For persistence:

Rules should be added to:
```bash
/etc/audit/rules.d/audit.rules
```
or managed using:
```bash
augenrules
```

---

## Noise and Performance Considerations
Potential impacts:
- High event volume from execve monitoring
- Increased disk usage
- SIEM ingestion cost

Mitigation:
- Filter by users or binaries
- Apply allowlists
- Reduce scope where appropriate
- Aggregate events

---

## Analyst Notes
- Audit rules must balance visibility and performance.
- Keys simplify hunting and correlation.
- Rules should evolve based on threat modeling.
- Periodic rule review is recommended.

