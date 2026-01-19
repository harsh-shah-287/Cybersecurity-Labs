# Detection Visualization Design – SOC Monitoring Perspective 

## Objective

To define how detection metrics and telemetry should be visualized within a SIEM dashboard to support real-time monitoring, rapid investigation, and operational decision-making.

Visualization transforms raw telemetry into actionable intelligence.

---

## Key Metrics to Visualize

The following metrics provide meaningful visibility for reconnaissance detection:

- SYN packets per second
- SYN packets per source IP
- Unique destination ports per source IP
- Alert frequency over time
- Top scanning sources
- Scan duration trends

These metrics directly support detection validation and threat monitoring.

---

## Dashboard View 1 – Network Activity Overview

### Visualization Type
- Line chart

### Metric Displayed
- Total SYN packets per minute

### Purpose
- Identify abnormal spikes in connection attempts.
- Visualize burst behavior and anomalies.

### Expected Pattern
- Baseline traffic shows steady low-volume activity.
- Scanning activity shows sharp spikes.

SOC Value:
- Early warning signal for reconnaissance.

---

## Dashboard View 2 – Top Talkers (Source IP Analysis)

### Visualization Type
- Bar chart

### Metric Displayed
- SYN packet count per source IP

### Purpose
- Identify dominant traffic generators.
- Detect suspicious sources quickly.

### Expected Pattern
- Normal traffic distributed across multiple hosts.
- Scanning activity dominated by a single host.

SOC Value:
- Rapid attribution and containment.

---

## Dashboard View 3 – Destination Port Diversity

### Visualization Type
- Histogram or bar chart

### Metric Displayed
- Unique destination ports contacted per source IP

### Purpose
- Identify multi-port probing behavior.
- Distinguish scanning from legitimate application traffic.

### Expected Pattern
- Baseline shows low port diversity.
- Scan shows hundreds of unique ports.

SOC Value:
- Strong behavioral indicator of reconnaissance.

---

## Dashboard View 4 – Alert Timeline

### Visualization Type
- Timeline chart

### Metric Displayed
- Detection alerts over time

### Purpose
- Identify alert clustering and recurrence.
- Monitor campaign behavior.

SOC Value:
- Trend analysis and operational planning.

---

## Dashboard View 5 – Correlated Signal View

### Visualization Type
- Table view

### Metric Displayed
- Network alert + endpoint event correlation

Columns Example:
- Timestamp
- Source IP
- SYN Count
- Unique Ports
- Sudo Event Time
- Alert Severity

Purpose:
- Provide investigative context in one view.
- Reduce analyst pivot time.

SOC Value:
- Faster triage and decision making.

---

## Alert Health Monitoring

Key operational indicators:

- Alerts per hour
- False positive rate
- Mean time to triage
- Repeat offender tracking

These metrics ensure detection quality and SOC sustainability.

---

## Visualization Design Principles

- Simplicity over complexity
- Clear labeling and legends
- Real-time visibility
- Drill-down capability
- Consistent time windows
- Minimal noise

Effective dashboards improve analyst performance.

---

## Investigation Workflow Using Dashboards

1. Detect spike in SYN volume.
2. Identify top source IP.
3. Validate port diversity.
4. Check correlated endpoint activity.
5. Escalate if confirmed malicious.

This mirrors real SOC workflows.

---

## Future Enhancements

- Geo-IP visualization
- Host reputation enrichment
- Trend comparison against historical baseline
- Automated severity scoring
- Integration with ticketing systems

---

## Analyst Notes

- Visualization accelerates human decision-making.
- Dashboards complement automated alerts.
- Poor visualization increases investigation time.
- Metrics must align with detection goals.

---
