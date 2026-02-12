# Detection Notes – Linux Audit Telemetry

## Objective

To document detection opportunities enabled by Linux audit telemetry and define behavioral logic that can be operationalized in SIEM or EDR platforms.

This document focuses on translating raw audit events into reliable security signals.

---

## Telemetry Sources

- Audit daemon (`auditd`)
- Rule keys:
  - `exec_monitor`
  - `sudo_watch`
- Query tools:
  - ausearch
  - aureport

---

## Detection Use Cases

---

### Use Case 1 – Suspicious Binary Execution

Objective:
Detect execution of potentially dangerous binaries commonly abused by attackers.

Target Binaries:
- nc
- ncat
- bash
- sh
- python
- perl
- curl
- wget

**Detection Logic:**
```sql
IF audit.key = exec_monitor
AND exe IN suspicious_binary_list
THEN generate alert 
```


Enrichment Fields:
- uid / euid
- tty
- cwd
- ppid
- command arguments

---

### Use Case 2 – Privileged Command Execution

Objective:
Detect commands executed with elevated privileges.

**Detection Logic:**
```sql
IF audit.key = exec_monitor
AND euid = 0
AND uid != 0
THEN generate alert
```

Risk:
- Privilege abuse
- Unauthorized administrative actions

---

### Use Case 3 – Repeated Command Execution (Automation)

Objective:
Detect automated or scripted activity.

**Detection Logic:**
```sql
COUNT(exec_monitor events) > 20 within 5 minutes by same uid
```


Risk:
- Malware
- Recon automation
- Brute-force tooling

---

### Use Case 4 – Sudo Configuration Tampering

Objective:
Detect modification attempts on sudo configuration.

**Detection Logic:**
```sql
IF audit.key = sudo_watch
AND perm IN (write, attribute)
THEN generate critical alert
```

Risk:
- Privilege persistence
- Insider threat
- Backdoor configuration

---

### Use Case 5 – Command Execution Outside Business Hours

Objective:
Detect abnormal timing behavior.

Detection Logic:
```sql
IF exec_monitor event occurs outside approved time window
THEN generate alert
```

Risk:
- Off-hours compromise

---

## Correlation Opportunities

Correlation increases confidence and reduces noise.

Examples:

- Privileged execution + network binary execution
- Repeated executions + sudo usage
- File modification + new process execution
- Unexpected user + sensitive binary

---

## False Positive Considerations

Common benign causes:

- System administrators
- Developers
- Automated jobs
- Maintenance scripts

Mitigation:

- User allowlisting
- Host role tagging
- Time-based filtering
- Command argument validation

---

## Alert Severity Guidance

| Condition | Severity |
|-------------|-------------|
| sudoers modification | Critical |
| Privileged network tool | High |
| Repeated suspicious execution | Medium |
| Single benign binary execution | Low |

---

## Detection Quality Metrics

Recommended metrics:

- Alert volume per day
- False positive rate
- Mean time to detection (MTTD)
- Analyst triage time
- Signal-to-noise ratio

---

## Analyst Notes

- Detection logic must be continuously tuned.
- Behavioral logic outperforms static signatures.
- Audit telemetry enables deep visibility.
- Correlation is essential for reliability.
- Documentation improves operational maturity.

---


