# Threat Hunting Findings – Network Reconnaissance Analysis 

## Hunt Objective

To validate the hypothesis that reconnaissance activity may be detectable through behavioral analysis of network telemetry and correlated endpoint logs, even when activity could potentially evade simple threshold-based detection.

---

## Hypothesis

A host may perform reconnaissance activity using SYN-based scanning techniques that can be identified through traffic behavior and correlation with endpoint activity.

Mapped MITRE technique : **T1046 - Network Service Discovery**


---

## Data Sources Reviewed

- Network Telemetry:
  - `nmap-syn-scan-capture-v2.pcapng`
  - `nmap-syn-scan-statistics.csv`
- Endpoint Telemetry:
  - systemd journal logs
- Derived Artifacts:
  - `metrics.md`
  - `thresholds.md`
  - `correlation.md`

---

## Analysis Summary

### Network Observations

| Observation | Result |
|---------------|---------|
| Source IP Distribution | Single host generated all SYN traffic |
| SYN Volume | 1686 SYN conversations |
| Destination Port Diversity | 843 unique ports |
| Scan Duration | ~50.5 seconds |
| Average SYN Rate | ~33 SYN/sec |
| TCP Handshake Completion | Majority incomplete |
| Traffic Pattern | Sustained burst |

Interpretation:
- Behavior strongly matches automated port scanning activity.
- High port diversity and burst rate exceed normal baseline behavior.

---

### Endpoint Observations

| Observation | Result |
|---------------|---------|
| Sudo Authentication Events | Present during scan window |
| Privileged Execution | Confirmed |
| Authentication Failures | None observed |
| System Errors | No critical errors detected |

Interpretation:
- Privileged activity aligns temporally with scanning execution.
- No evidence of brute force or unauthorized access.

---

### Correlation Analysis

- Network scan activity and sudo authentication events occurred within a short temporal window.
- Sequence observed: Privilege escalation → Network scanning activity → Termination.
- Correlation increases confidence that the scanning activity was intentional and tool-driven.

---

## Hypothesis Evaluation

| Criteria | Outcome |
|------------|----------|
| Behavioral scan indicators | Confirmed |
| Endpoint correlation | Confirmed |
| Evasion behavior detected | Not observed |
| Stealth scanning behavior | Not observed |

Conclusion:
- Hypothesis partially validated.
- Reconnaissance behavior was clearly observable but not stealthy in this dataset.

---

## Risk Assessment

| Factor | Assessment |
|------------|------------|
| Attack Stage | Reconnaissance |
| Impact | Low (lab environment) |
| Likelihood | Controlled testing |
| Exploitation Risk | None |

If this activity occurred in production:
- Would represent early-stage attacker reconnaissance.
- Would warrant investigation and containment.

---

## Notable Observations

- Port diversity exceeded typical enterprise baseline by a large margin.
- Packet rate remained consistent across scan window.
- No evidence of distributed scanning.
- No lateral movement detected.

---

## Evidence References

- PCAP: `nmap-syn-scan-capture-v2.pcapng`
- CSV Statistics: `nmap-syn-scan-statistics.csv`
- Log Exports: `log-window.log`


---

## Analyst Notes

- Dataset represents controlled lab behavior.
- Metrics are suitable for validating detection logic.
- Additional hunts should focus on stealth techniques.
- Correlation significantly improves confidence.