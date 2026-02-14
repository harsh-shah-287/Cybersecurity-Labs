# Detection Correlation Logic – Network and Endpoint Telemetry 
---

## Objective

To design multi-signal correlation logic that increases detection confidence by combining network-based reconnaissance indicators with endpoint privilege activity.

Correlation reduces false positives and improves investigative accuracy.

---

## Telemetry Sources

### Network Telemetry
- Source:
  - PCAP captures
  - Flow statistics
- Signals:
  - High volume of TCP SYN packets
  - High destination port diversity
  - Sustained burst behavior
- Detection Source:
  - Threshold-based SYN scan detection

---

### Endpoint Telemetry
- Source:
  - systemd journal logs
- Signals:
  - sudo authentication events
  - Privileged command execution
- Detection Source:
  - journalctl log filtering

---

## Primary Signals

### Signal A – Network Reconnaissance

Triggered when:

- > 500 SYN packets in ≤ 60 seconds  
- > 100 unique destination ports  
- SYN rate > 10 packets/sec  

Represents:
- Port scanning
- Service discovery
- Reconnaissance behavior

---

### Signal B – Privilege Activity

Triggered when:

- sudo authentication event occurs
- Privileged command executed successfully

Represents:
- Elevated system access
- Potential attacker preparation or tooling execution

---

## Correlation Window

- Time window for correlation: ± 5 minutes
- Signals occurring within this window are considered related.

---

## Correlation Logic

- If Signal A (Network Scan) occurs AND Signal B (Privilege Activity) occurs within ±5 minutes THEN Raise High-Confidence Security Alert.

---

## Alert Confidence Levels

| Condition | Confidence |
|------------|-------------|
| Signal A only | Medium |
| Signal B only | Low |
| Signal A + Signal B correlated | High |

---

## Investigation Context Added

Correlation enriches alerts with:

- Source IP attribution
- Timestamp alignment
- User privilege context
- Activity sequencing
- Behavioral intent

This reduces triage time and improves analyst efficiency.

---

## False Positive Considerations

Potential benign correlation scenarios:

- Administrator performing network troubleshooting
- Authorized vulnerability scanning
- Lab testing activity

Mitigation:

- Maintain asset and user context
- Whitelist approved scanners
- Apply correlation only to non-admin endpoints

---

## False Negative Risks

Potential evasion scenarios:

- Attacker uses non-privileged scanning tools
- Delayed privilege escalation
- Distributed scanning techniques

Mitigation:

- Add additional signals:
  - Authentication failures
  - Process execution telemetry
  - Network anomaly detection

---

## Scalability Considerations

Future correlation expansion may include:

- EDR process telemetry
- Firewall logs
- DNS telemetry
- Authentication failures
- User behavior analytics

---

## Enhanced Endpoint Correlation (Auditd)

New Telemetry Source:
- auditd execve monitoring

Additional Correlation Logic:

If : `Network SYN scan detected` and `audit.key = exec_monitor event contains suspicious binary (nmap, nc, curl) Within ±5 minutes THEN escalate alert to Critical.`

---

## Analyst Notes

- Correlation improves detection fidelity.
- Multi-signal logic reduces alert fatigue.
- Time normalization is critical for accuracy.
- Correlation rules must evolve with environment behavior.

---