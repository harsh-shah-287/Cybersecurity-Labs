# Lessons Learned – Detection Improvement and Hunting Maturity 

## Objective

To capture insights gained from the threat hunting exercise and translate them into actionable improvements for detection logic, telemetry coverage, and operational maturity.

Threat hunting should continuously strengthen defensive posture.

---

## Summary of Key Findings

- Reconnaissance behavior was clearly observable through:
  - High SYN volume
  - High destination port diversity
  - Sustained burst traffic
  - Incomplete TCP handshakes
- Network and endpoint telemetry correlated successfully.
- Detection thresholds would have triggered reliably for this activity.
- No stealth or evasion behavior was observed in this dataset.

---

## Detection Gaps Identified

### 1. Limited Coverage for Low-and-Slow Scanning

Current detection focuses on:
- High-volume
- Short-duration scanning

Gap:
- Slow scanning spread over long time windows may evade thresholds.

Improvement:
- Add long-window aggregation detection (e.g., 1–6 hours).
- Monitor cumulative port diversity over time.

---

### 2. Lack of Process-Level Visibility

Current telemetry includes:
- Network traffic
- Sudo authentication logs

Gap:
- No visibility into specific processes or command execution details.

Improvement:
- Add endpoint telemetry such as:
  - Process creation logs
  - Command-line auditing
  - Sysmon or auditd telemetry

---

### 3. Limited User Context

Current correlation identifies sudo usage but lacks:
- User intent
- Asset role classification
- Authorized activity context

Improvement:
- Maintain asset inventory and ownership mapping.
- Integrate user identity context into detections.

---

## Threshold Optimization Opportunities

- Current thresholds detect aggressive scanning well.
- Thresholds may need adjustment in larger networks with legitimate scanners.

Improvements:
- Implement adaptive thresholds based on baseline trends.
- Separate thresholds by asset class (server vs workstation).
- Apply rate-based smoothing to reduce noise.

---

## Correlation Enhancement Opportunities

Enhance confidence by correlating additional signals:

- Authentication failures
- Process execution anomalies
- DNS anomalies
- EDR alerts
- Firewall deny logs

Multi-signal correlation improves detection fidelity.

---

## Telemetry Expansion Recommendations

To strengthen hunting capability:

- Deploy endpoint telemetry collection (Sysmon / auditd).
- Centralize log collection.
- Enable network flow monitoring.
- Retain historical telemetry for trend analysis.

---

## Operational Improvements

- Standardize hunt documentation templates.
- Schedule periodic hunting cycles.
- Track hunt outcomes and metrics.
- Feed hunt results back into detection engineering.

---

## Skills Developed

- Hypothesis-driven analysis
- Behavioral detection thinking
- Telemetry validation
- Correlation reasoning
- Documentation discipline
- SOC investigation methodology

---

## Next Hunting Opportunities

Future hunts may explore:

- DNS tunneling behavior
- Credential brute force patterns
- Lateral movement indicators
- Living-off-the-land techniques
- Persistence mechanisms

---

## Analyst Notes

- Threat hunting complements automated detection.
- Continuous improvement increases security maturity.
- Documentation enables scalability and knowledge transfer.
- Hunting mindset strengthens Purple Team capability.

