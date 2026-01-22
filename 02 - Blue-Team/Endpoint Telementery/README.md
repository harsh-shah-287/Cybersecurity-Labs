# Endpoint Telemetry and Host-Based Detection Lab 

## Objective

To understand how endpoint telemetry provides high-fidelity visibility into system activity and how host-based detections can be engineered using process execution, privilege escalation, and behavioral indicators.

This lab simulates attacker tradecraft and converts observed telemetry into structured detection logic.

---

## Environment

- OS: Kali Linux (VMware)
- User Context: kali (UID 1000)
- Telemetry Sources:
  - Process execution (`ps`, `top`)
  - Privilege logs (`journalctl`)
- Tools Used:
  - Netcat
  - curl
  - bash
  - Linux CLI utilities
  - Git

---

## Lab Structure

- Endpoint-Telemetry/
- ├── README.md
- ├── process-baseline.md
- ├── suspicious-activity.md
- └── detection-logic.md


---

## Baseline Establishment

Normal endpoint behavior was captured shortly after system boot using:

```bash
ps aux | head -20
top -b -n 1 | head -20
```

**Baseline observations included:**
- Dominance of kernel worker threads.
- Minimal active user processes.
- Low CPU utilization (~90% idle).
- Healthy memory usage and zero swap pressure.
- No persistent network listeners.
- Minimal privileged execution.

Baseline documentation: `process-baseline.md`

---

## Suspicious Activity Simulation
Controlled activities were executed to simulate attacker techniques:
- Privileged netcat listener (sudo nc -lvnp 4444)
- Interactive netcat session
- Privileged outbound network retrieval (sudo curl)
- Privileged file creation in /tmp

Observed telemetry confirmed:
- Multiple sudo privilege escalations.
- Execution of LOLBins under root context.
- Temporal clustering of suspicious activity.
- Command evidence captured in system logs.

Detailed analysis: `suspicious-activity.md`

---

## Detection Logic Development
Based on observed telemetry, detection logic was engineered to identify:
- Privileged netcat listeners.
- Privileged network utility execution.
- Suspicious temporary file creation.
- Repeated sudo escalation patterns.
- Correlation between multiple endpoint signals.

Detection logic includes:
- Signal definitions
- Correlation rules
- Severity classification
- False positive handling
- Validation strategy

Detection documentation: `detection-logic.md`

---

## Key Learnings
- Endpoint telemetry provides visibility beyond network data.
- Behavioral analysis improves detection accuracy.
- LOLBin abuse is a common attacker technique.
- Correlation reduces alert noise.
- Baselines are essential for anomaly detection.
- Detection engineering requires iterative tuning.

---

## Detection Engineering Alignment
This lab supports:
- SOC analyst skill development
- Detection engineering fundamentals
- Purple Team methodology
- MITRE ATT&CK mapping
- Operational documentation discipline

---

## Evidence
- Baseline Data: `process-baseline.md`
- Suspicious Activity Analysis: `suspicious-activity.md`
- Detection Logic: `detection-logic.md`
