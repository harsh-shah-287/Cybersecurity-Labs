# SIEM Query Logic and Detection Visualization Simulation Lab 

## Objective

To simulate how a Security Information and Event Management (SIEM) platform ingests telemetry, executes detection queries, aggregates metrics, and visualizes security signals for SOC analysts.

This lab translates previously engineered detection logic into:
- Query modeling
- Metric aggregation
- Behavioral analysis
- Visualization design
- Operational SOC workflows

---

## Environment

- OS: Kali Linux (VMware)
- Data Sources:
  - `nmap-syn-scan-statistics.csv` (Wireshark TCP Conversations export)
- Tools Used:
  - Linux CLI utilities (`cut`, `sort`, `uniq`, `wc`)
  - Text editor
  - Git

---

## Lab Structure
- SIEM-Simulation/
- ├── README.md
- ├── queries.md
- ├── metrics-analysis.md
- └── visualization-notes.md


---

## Data Reference

- Source PCAP: `nmap-syn-scan-capture-v2.pcapng`
- Statistics Export: `nmap-syn-scan-statistics.csv`
- Filter applied in wireshark: `tcp.flags.syn == 1 && tcp.flags.ack == 0`

---

## Tasks Performed

### 1. Query Logic Modeling
- Translated detection thresholds into platform-agnostic pseudo queries.
- Designed filtering, aggregation, rate detection, and correlation logic.
- Simulated how SIEM engines convert telemetry into alerts.

File: `queries.md`

---

### 2. Metric Aggregation and Validation
- Parsed CSV data using Linux command-line utilities.
- Aggregated:
  - Source IP distribution
  - Destination port cardinality
  - SYN conversation volume
- Validated telemetry integrity and consistency.

Key Commands Used:
```bash
tail -n +2 nmap-syn-scan-statistics.csv | cut -d',' -f1 | tr -d '"' | sort | uniq -c
tail -n +2 nmap-syn-scan-statistics.csv | cut -d',' -f4 | sort | uniq | wc -l
```

- File: `metrics-analysis.md`

### 3. Behavioral Interpretation

- Analyzed traffic patterns and anomaly characteristics.
- Compared scan behavior against expected baseline behavior.
- Mapped observations to detection engineering principles.

### 4. Virtualization Design
Designed SOC dashboard concepts for:

- SYN volume monitoring
- Top talker identification
- Port diversity analysis
- Alert timelines
- Correlated signal views
- Defined how analysts would consume telemetry operationally.

File: `visualization-notes.md`

## Extracted Metrics Summary
| Metric | Observed Value |
|--------|----------------|
| Source IP | 192.168.15.129 |
| Total SYN Conversations | 1686 |
| Unique Destination Ports | 843 |
| Scan Duration | ~50.5 seconds |
| Average SYN Rate | ~33 SYN/sec |
| Target Hosts | Single destination |

---

## Detection Engineering Alignment
This lab reinforces:
- Translating detection logic into query models
- Metric-driven detection validation
- Threshold reasoning
- SOC operational visualization thinking
- Correlation mindset

---

## Key Learnings
- SIEM systems operate on filtered, indexed telemetry.
- Aggregation and grouping drive detection logic.
- Metrics validate behavioral anomalies.
- Visualization improves analyst efficiency and response speed.
- Clean data parsing is critical for reliable analytics.
- Detection engineering extends beyond tools into reasoning.

---

## Future Enhancements
- Implement real SIEM platform queries (Splunk / Elastic).
- Automate dashboard generation.
- Integrate endpoint telemetry.
- Add alert replay testing.
- Expand KPI tracking.

---

## Evidence 
- Detection Queries: `queries.md` 
- Metrics Analysis: `metrics-analysis.md`
- Visualization Design: `visualization-notes.md`
