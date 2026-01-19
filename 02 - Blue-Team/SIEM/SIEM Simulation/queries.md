# SIEM Query Logic – Detection Engineering Simulation 


## Objective

To translate detection requirements and threshold logic into platform-agnostic SIEM query structures that simulate how real security monitoring systems filter, aggregate, and alert on telemetry.

This document models how detection logic would be expressed in query form.

---

## Data Source Reference

Input dataset: `nmap-syn-scan-statistics.csv`

Telemetry fields conceptually represented:

- timestamp
- source_ip
- destination_ip
- destination_port
- tcp_flag
- packet_count
- duration

---

## Query 1 – Identify All TCP SYN Traffic

Purpose:
Filter all initial connection attempts.

Pseudo Query: `SELECT *
FROM network_logs
WHERE tcp_flag = "SYN" AND tcp_ack = false`


SIEM Behavior:
- Filters raw traffic into relevant signal set.
- Removes noise from non-SYN packets.

---

## Query 2 – Count SYN Packets Per Source IP Per Time Window

Purpose:
Measure scan intensity.

Pseudo Query: `SELECT source_ip, COUNT(*) AS syn_count
FROM network_logs
WHERE tcp_flag = "SYN"
GROUP BY source_ip, 1-minute window`


SIEM Behavior:
- Aggregates traffic into measurable metrics.
- Enables threshold evaluation.

---

## Query 3 – Identify Destination Port Cardinality

Purpose:
Detect port scanning behavior.

Pseudo Query: `SELECT source_ip, COUNT(DISTINCT destination_port) AS unique_ports
FROM network_logs
WHERE tcp_flag = "SYN"
GROUP BY source_ip, 1-minute window`


SIEM Behavior:
- Detects multi-port probing.
- Distinguishes scanning from normal browsing.

---

## Query 4 – Detect Threshold Breach (Alert Logic)

Purpose:
Trigger detection when thresholds exceeded.

Pseudo Query: `SELECT source_ip
FROM network_logs
WHERE tcp_flag = "SYN"
GROUP BY source_ip, 1-minute window
HAVING COUNT(*) > 500
AND COUNT(DISTINCT destination_port) > 100`


SIEM Behavior:
- Converts metrics into alert logic.
- Generates detection events.

---

## Query 5 – Rate-Based Detection

Purpose:
Detect burst behavior.

Pseudo Query: `SELECT source_ip, COUNT(*) / window_duration AS syn_rate
FROM network_logs
WHERE tcp_flag = "SYN"
GROUP BY source_ip, 60-second window
HAVING syn_rate > 10`


SIEM Behavior:
- Measures packet velocity.
- Detects abnormal bursts.

---

## Query 6 – Correlate Network Scan with Endpoint Activity

Purpose:
Increase alert confidence.

Pseudo Query: `SELECT *
FROM network_alerts N
JOIN endpoint_logs E
ON N.source_ip = E.host_ip
WHERE E.event_type = "sudo_execution"
AND ABS(N.timestamp - E.timestamp) <= 5 minutes`


SIEM Behavior:
- Correlates multi-source telemetry.
- Elevates alert severity.

---

## Query 7 – Baseline Comparison

Purpose:
Compare current activity against baseline behavior.

Pseudo Query: `SELECT source_ip,
CURRENT.syn_rate,
BASELINE.avg_syn_rate
FROM network_metrics CURRENT
JOIN baseline_metrics BASELINE
ON CURRENT.source_ip = BASELINE.source_ip
WHERE CURRENT.syn_rate > (BASELINE.avg_syn_rate * 5)`


SIEM Behavior:
- Detects anomalies dynamically.
- Adapts to environment patterns.

---

## Query Design Principles

- Filter early to reduce noise.
- Aggregate consistently.
- Use time windows effectively.
- Prefer behavior over indicators.
- Correlate signals for confidence.
- Validate against baseline data.

---

## Analyst Notes

- These queries are conceptual and platform-neutral.
- Real implementations vary by SIEM vendor syntax.
- Logical structure remains consistent across platforms.
- Query performance must be considered at scale.

---


