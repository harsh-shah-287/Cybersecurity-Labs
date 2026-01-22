# Endpoint Detection Logic – Privileged LOLBin Abuse 

## Objective

To define host-based detection logic that identifies suspicious execution of Living-Off-The-Land binaries (LOLbins) under elevated privileges and correlates them with network behavior and repeated privilege escalation.

This detection logic is derived from real endpoint telemetry observed during lab simulations.

---

## Detection Scope

This detection focuses on:

- Privileged execution of network utilities
- Persistent or repeated sudo sessions
- Temporary file creation under root context
- Netcat listener behavior
- Correlation with network activity

---

## Primary Threat Model

| Category | Description |
|-------------|----------------|
| Attack Technique | LOLBin abuse |
| Initial Access | Local execution |
| Privilege Escalation | sudo |
| Command & Control | Netcat listener |
| Payload Staging | curl + /tmp write |

---

## MITRE ATT&CK Mapping

| Tactic | Technique |
|------------|------------------------------|
| Discovery | T1046 – Network Service Discovery |
| Command and Control | T1105 – Ingress Tool Transfer |
| Execution | T1059 – Command-Line Interface |
| Persistence | T1053 – Scheduled Task (potential) |
| Privilege Escalation | T1548 – Abuse Elevation Control |

---

## Signal Definitions

### Signal A – Privileged Netcat Listener

Trigger when:

- Process name = `nc`
- Command line contains:
  - `-l` OR `--listen`
  - `-p` OR port binding
- Executed under user = root
- Parent process = sudo

Risk:
- Backdoor creation
- Reverse shell staging

---

### Signal B – Privileged Network Utility Execution

Trigger when:

- Process name = `curl` OR `wget`
- Executed under root
- External network destination accessed

Risk:
- Payload retrieval
- Data exfiltration

---

### Signal C – Privileged Temporary File Write

Trigger when:

- Process name = `bash`
- Command line contains:
  - `/tmp`
- Executed under root

Risk:
- Payload staging
- Persistence preparation

---

### Signal D – Repeated Privilege Escalation

Trigger when:

- > 3 sudo sessions in ≤ 10 minutes
- Same user initiating escalation

Risk:
- Automated tooling
- Post-compromise activity

---

## Correlation Logic
**If (Signal A OR Signal B OR Signal C) AND Signal D occurs within ±10 minutes THEN Raise High-Severity Endpoint Alert**

---

## Alert Severity Classification

| Condition | Severity |
|------------|-------------|
| Signal A alone | High |
| Signal B or C alone | Medium |
| Multiple signals correlated | Critical |
| Signal D only | Low |

---

## False Positive Considerations

Legitimate scenarios:

- Network troubleshooting
- Administrative testing
- Penetration testing labs
- Development environments

Mitigation:

- Asset role filtering
- User allowlisting
- Time-of-day constraints
- Command argument validation

---

## False Negative Risks

Potential evasion:

- Renamed binaries
- Non-privileged execution
- Short-lived execution
- Encrypted tunnels

Mitigation:

- Hash-based monitoring
- Behavior-based detection
- Parent-child analysis
- Network correlation

---

## Detection Validation Strategy

Validation methods:

- Replay simulated commands
- Verify alert triggering
- Measure alert volume
- Review false positives
- Tune thresholds

---

## Operational Deployment Considerations

- Ensure endpoint telemetry coverage exists.
- Validate log ingestion reliability.
- Monitor performance impact.
- Maintain tuning documentation.
- Review detections periodically.

---

## Analyst Notes

- Detection is behavior-based, not signature-based.
- Correlation improves confidence and reduces noise.
- Endpoint telemetry provides high-fidelity detection.
- Continuous tuning is required for production environments.