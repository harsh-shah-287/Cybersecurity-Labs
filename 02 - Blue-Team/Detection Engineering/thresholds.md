# Threshold Engineering – TCP SYN Scan Detection 

---

## Objective

To define measurable, data-driven thresholds for detecting TCP SYN port scanning activity while balancing detection sensitivity and false positive risk.

Thresholds are derived from real telemetry captured during controlled scanning activity and compared conceptually against expected baseline behavior.

---

## Reference Metrics (Observed Scan)

Source: `nmap-syn-scan-statistics.csv`


Observed values:

| Metric | Value |
|--------|-------|
| Total SYN Events | 1686 |
| Unique Destination Ports | 843 |
| Scan Duration | ~50.5 seconds |
| Average SYN Rate | ~33 SYN/sec |
| Target Hosts | Single destination |
| Traffic Pattern | Sustained burst |

These values represent aggressive reconnaissance behavior.

---

## Baseline Assumptions

Expected baseline characteristics during normal user activity:

- SYN packet volume is low and sporadic.
- Destination ports are limited (typically 2–10 ports).
- TCP handshakes normally complete.
- Packet rate remains low (<2 SYN/sec).
- No sustained burst behavior.

Baseline values should always be validated using real environment captures.

---

## Threshold Selection Strategy

Thresholds are designed to:

- Trigger reliably on scanning behavior.
- Avoid triggering on normal browsing or application activity.
- Maintain operational usability.

We intentionally select thresholds significantly below observed scan metrics but above baseline behavior.

---

## Primary Detection Thresholds

### Threshold 1 – SYN Volume

- Alert if > 500 SYN packets observed within 60 seconds

Justification:
- Observed scan: ~1686 SYN in ~50 seconds
- Baseline: typically <100 SYN per minute
- Provides strong separation between benign and malicious behavior.

---

### Threshold 2 – Destination Port Cardinality

- Alert if > 100 unique destination ports contacted within 60 seconds

Justification:
- Observed scan: 843 ports
- Normal traffic rarely exceeds 10–20 ports per minute
- High port diversity strongly indicates scanning.

---

### Threshold 3 – Packet Rate

- Alert if average SYN rate > 10 packets per second

Justification:
- Observed scan: ~33 SYN/sec
- Baseline traffic typically <2 SYN/sec
- Rate threshold captures burst behavior.

---

## Composite Alert Logic

- To reduce false positives, combine thresholds: 
- Trigger alert when : SYN volume threshold, Port cardinality threshold AND Rate threshold are met within same time window.
- This ensures high-confidence detection.

## Correlation Enhancement 

- Increase confidence by correlating with endpoint telementary: If network scan detected AND sudo execution occurs within ±5 minutes → Elevate alert severity.

---

## False Positive Considerations

Potential benign sources:

- Vulnerability scanners
- Network discovery tools
- Monitoring systems
- Authorized penetration tests

Mitigation strategies:

- Whitelist known scanner IPs.
- Apply detection only to user workstations.
- Adjust thresholds based on environment size.

---

## False Negative Risks

Potential evasion scenarios:

- Slow scanning techniques
- Distributed scanning across multiple IPs
- Randomized timing

Mitigation strategies:

- Add long-window aggregation detection.
- Monitor low-and-slow behavior separately.
- Correlate with other telemetry sources.

---

## Tuning Strategy

Thresholds should be:

1. Validated against baseline captures.
2. Adjusted incrementally.
3. Reviewed periodically.
4. Tuned based on alert volume and accuracy.

Detection quality improves through continuous iteration.

---

## Analyst Notes

- Accurate filtering is critical for metric reliability.
- Conversation-level metrics reduce packet noise.
- Thresholds must remain environment-specific.
- Correlation improves detection confidence.

---



