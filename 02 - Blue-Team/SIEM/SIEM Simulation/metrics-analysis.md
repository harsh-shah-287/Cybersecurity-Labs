# Metrics Analysis – SYN Scan Telemetry 


## Objective

To analyze extracted telemetry metrics from TCP conversation statistics and interpret behavioral patterns relevant to detection engineering and SOC monitoring.

This analysis converts raw counts into operational insights.

---

## Data Source

- Dataset: `nmap-syn-scan-statistics.csv`
- Source: Wireshark → Statistics → Conversations → TCP
- Filter Applied: `tcp.flags.syn == 1 && tcp.flags.ack == 0`


---

## Extracted Metrics Summary

| Metric | Observed Value |
|--------|----------------|
| Source IP | **192.168.15.129**|
| Total SYN Conversations | **1686** |
| Unique Destination Ports | **843** |
| Target Hosts | **Single destination** |
| Scan Duration | **~50.5 seconds** |
| Average SYN Rate | **~33 SYN/sec** |

---

## Source Distribution Analysis

Command Used:

```bash
tail -n +2 nmap-syn-scan-statistics.csv | cut -d',' -f1 | tr -d '"' | sort | uniq -c
``` 
**Result:**
- All SYN traffic originated from a single host.
- No distributed sources observed.

**Interpretation:**
- Behavior matches a centralized scanning tool rather than background noise or multi-host traffic.

**SOC Relevance:**
- Single-source concentration simplifies attribution and response.

---

## Destination Port Analysis

Commands used :

```bash
tail -n +2 nmap-syn-scan-statistics.csv | cut -d',' -f4 | sort | uniq | wc -l
```

**Result:**
- 843 unique destination ports targeted.

**Interpretation:**
- Extremely high port diversity.
- Normal client traffic rarely exceeds 10–20 ports in short time windows.

**SOC Relevance:**
- High port cardinality is a strong indicator of reconnaissance.

---

## Traffic Volume and Rate Analysis

**Derived Metrics:**
- Total SYN events: **1686**
- Duration: **~50.5 seconds**
- Average rate: **~33 SYN packets per second**

**Interpretation:**
- Sustained high packet rate indicates automated scanning.
- Burst pattern is inconsistent with normal user behavior.

**SOC Relevance:**
- Rate-based thresholds provide reliable detection signals.

---

## Behavioral Pattern Summary

Observed behaviors:
- Rapid SYN packet generation
- High destination port diversity
- Minimal bidirectional communication
- No sustained TCP sessions
- Structured probing pattern

These behaviors align with network service discovery activity.

**MITRE Mapping:** `T1046 – Network Service Discovery`

---

## Baseline Contrast

Expected baseline behavior:
- Low SYN volume
- Limited destination port variety
- Completed TCP handshakes
- No burst patterns

Observed telemetry deviates significantly from baseline expectations.

--- 

## Detection Engineering Implications

The extracted metrics support:
- Threshold-based detection logic
- Rate-based alerting
- Cardinality-based anomaly detection
- Correlation with endpoint telemetry

Metrics provide quantitative justification for alert thresholds.

## Analyst Notes
- Metrics were derived using deterministic aggregation.
- Data integrity verified through multiple validation steps.
- Results suitable for production-style detection modeling.
- Thresholds should be validated against baseline traffic.