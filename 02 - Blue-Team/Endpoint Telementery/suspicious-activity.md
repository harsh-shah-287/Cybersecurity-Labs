# Suspicious Activity Analysis – Endpoint Telemetry 

## Objective

To observe, document, and analyze endpoint behavior that deviates from established baseline activity by intentionally executing network and system utilities that simulate attacker tradecraft.

This exercise focuses on identifying Living-Off-The-Land (LOLBin) abuse, privilege escalation telemetry, and suspicious execution patterns.

---

## Data Sources

- Process Telemetry:
  - `ps aux`
- Privilege Telemetry:
  - systemd journal (`journalctl`)
- User Context:
  - kali (UID 1000)

---

## Simulated Activities Executed

The following commands were executed intentionally to generate suspicious telemetry:

### 1. Privileged Netcat Listener

```bash
sudo nc -lvnp 4444
``` 
**Observed Log Evidence:**
```bash
Jan 21 07:39:36 kali sudo[3554]: COMMAND=/usr/bin/nc -lvnp 4444 
Jan 21 07:46:43 kali sudo[7727]: COMMAND=/usr/bin/nc -lvnp 4444
```
**Behavior Observed:**
- Netcat executed under root privileges.
- Listener bound to port 4444.
- Session remained open for multiple minutes before termination.
- Multiple execution attempts observed.

**Deviation from Baseline:**
- No persistent listeners present in baseline.
- Introduction of externally reachable service is anomalous.

Security Risk:
- Netcat listeners are commonly used for backdoors and reverse shells.
---
### 2. Elevated Network Retrieval (curl)
```bash
sudo curl http://example.com
```
Observed Log Evidence:
```bash
Jan 21 07:42:05 kali sudo[4931]: COMMAND=/usr/bin/curl http://example.com
Jan 21 07:54:25 kali sudo[11505]: COMMAND=/usr/bin/curl http://example.com
```
**Behavior Observed:**
- Network utility executed with elevated privileges.
- Outbound HTTP connection initiated.
- Repeated executions detected.

**Deviation from Baseline:**
- Baseline showed no privileged outbound utilities.

**Security Risk:**
- Privileged download tools may indicate payload retrieval or staging.
---
### 3. Privileged File Modification via Bash
```bash
sudo bash -c 'echo test > /tmp/testfile'
```
Observed Log Evidence:
```bash
Jan 21 07:56:48 kali sudo[12751]: COMMAND=/usr/bin/bash -c 'echo test > /tmp/testfile'
```
**Behavior Observed:**
- Bash executed with elevated privileges.
- File created in temporary directory.

**Deviation from Baseline:**
- No privileged file creation observed during baseline.

**Security Risk:**
- Temporary directories are frequently used for payload staging and persistence.
---
### 4. Repeated Privilege Sessions
Observed Log Evidence: 
- Multiple sudo session open/close cycles:
```bash
session opened for user root(uid=0) by kali(uid=1000)
session closed for user root
```
**Behavior Observed:**
- Multiple privilege escalation events in short time window.
- Commands executed across different terminal sessions (pts/0, pts/1, pts/3).

**Deviation from Baseline:**
- Baseline showed minimal privileged activity.

**Security Risk:**
- Repeated elevation attempts may indicate attacker activity.
---
##  Process Visibility Observations
Process snapshot command:
```bash
ps aux | grep "nc|curl|bash"
```
**Observed Result:**
- No long-running suspicious processes persisted at capture time.
- Netcat processes terminated after testing.
- Only grep process remained visible.

**Interpretation:**
- Activity was transient.
- Highlights importance of continuous telemetry collection in production.
---
## Behavioral Timeline Summary
| Time | Activity |
| ---- | -------- |
| 07:39 | Netcat listener executed |
| 07:42	| Curl executed |
| 07:46	| Netcat executed again |
| 07:54	| Curl executed |
| 07:56	| Privileged bash file creation |
| 07:58	| journalctl executed |

**Pattern:**
- Repeated privilege usage.
- Network utilities clustered in short window.
- Consistent behavioral sequencing.
---

## Detection-Relevant Indicators
High-confidence signals:
- Execution of /usr/bin/nc with -l (listener) flags under sudo.
- Repeated privileged execution within short timeframe.
- Privileged network utilities (curl) executed.
- Bash used for file creation under elevated context.
- Multiple TTY sessions executing elevated commands.

---

## False Positive Considerations
Legitimate scenarios:
- System administration
- Network troubleshooting
- Lab environments
- Development testing

Mitigation Strategies:
- Apply asset role classification.
- Correlate with network telemetry.
- Alert only on non-admin endpoints.
- Apply frequency thresholds.

---

## Analyst Notes
- All activity performed intentionally in a lab environment.
- Telemetry demonstrates realistic attacker tradecraft patterns.
- Endpoint logs provide high-fidelity detection opportunities.
- Short-lived processes emphasize need for continuous monitoring.
- Evidence-driven documentation improves detection quality.