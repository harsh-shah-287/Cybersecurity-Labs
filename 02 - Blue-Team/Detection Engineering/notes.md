# Detection Engineering Notes – Foundations and Practical Telemetry (Day 10)

---

## What Is Detection Engineering?

Detection engineering is the discipline of designing, building, validating, and continuously improving rules that detect malicious or suspicious behavior across systems, networks, and endpoints.

Instead of relying purely on static indicators (IP addresses, hashes, domains), detection engineering focuses on behavioral patterns that are difficult for attackers to evade.

Detection lifecycle:

Data → Signal → Rule → Alert → Tuning → Response → Improvement

---

## Indicator-Based vs Behavior-Based Detection

### Indicator-Based Detection
- Matches known bad values:
  - Malicious IP addresses
  - File hashes
  - Domains
- Advantages:
  - Simple to implement
  - Quick initial protection
- Limitations:
  - Easily bypassed
  - Requires constant updates
  - Short operational lifespan

### Behavior-Based Detection
- Detects attacker activity patterns:
  - Port scanning behavior
  - Abnormal authentication attempts
  - Suspicious process execution
- Advantages:
  - Harder to evade
  - Detects unknown threats
  - Longer detection lifespan
- Limitations:
  - Requires tuning
  - May generate false positives initially

Modern SOC operations prioritize behavior-based detection.

---

## SOC Telemetry Sources

Common data sources used for detection:

- Network:
  - PCAP
  - NetFlow
  - Firewall logs
- Endpoint:
  - Windows Event Logs
  - Sysmon
  - Linux audit and journal logs
- Authentication:
  - SSH logs
  - sudo logs
  - Active Directory logs
- Cloud:
  - AWS CloudTrail
  - Azure Activity Logs
- Application:
  - Web server logs
  - API logs
- Security Controls:
  - IDS / IPS
  - EDR
  - SIEM platforms

Multiple telemetry sources increase detection confidence and investigation accuracy.

---

## MITRE ATT&CK Overview

MITRE ATT&CK is a standardized framework describing real-world attacker behavior.

- Tactic = Why the attacker performs an action  
- Technique = How the attacker performs it  

Example:

- Tactic: Discovery  
- Technique: T1046 – Network Service Discovery  

Mapping detections to MITRE:
- Standardizes coverage tracking
- Improves communication across teams
- Enables gap analysis

---

## Sigma Rules Overview

Sigma is a generic detection rule format that can be converted into SIEM queries.

Core components:
- title
- description
- logsource
- detection
- condition
- falsepositives
- level

Sigma allows detections to remain platform-agnostic.

---

## Signal vs Noise

Important detection concepts:

- Signal:
  - True malicious or suspicious behavior
- Noise:
  - Normal or benign activity
- False Positive:
  - Legitimate behavior incorrectly flagged
- False Negative:
  - Malicious behavior missed by detection

Detection quality depends on:
- Accurate baselines
- Proper thresholds
- Context enrichment
- Continuous tuning

---

## Practical Telemetry Generated

During lab execution:

- TCP SYN packets captured: **2811**
- Scan type: TCP SYN scan (`nmap -sS`)
- Endpoint logs generated: sudo authentication events
- Evidence saved: PCAP and journal log window

This dataset represents real reconnaissance behavior.

---

## Command Purpose and SOC Relevance

### `sudo nmap -sS <target>`
- Generates half-open TCP SYN scans.
- Produces high-volume SYN traffic.
- Represents attacker reconnaissance behavior.
- SOC Detection Use:
  - Detect port scanning
  - Identify reconnaissance attempts
  - Map to MITRE T1046

---

### Wireshark Capture on `eth0`
- Captures raw network packets.
- Places interface into promiscuous mode.
- Provides ground truth telemetry.
- SOC Equivalent:
  - Network sensors
  - IDS taps
  - SPAN ports

---

### Wireshark Filter
- `tcp.flags.syn == 1 && tcp.flags.ack == 0`

- Isolates initial connection attempts.
- Removes unrelated noise.
- Enables packet counting and behavior analysis.
- SOC Equivalent:
  - SIEM query filtering
  - IDS signature filtering

---

### Wireshark Statistics – TCP Conversations
- Aggregates packet counts and flow behavior.
- Extracts metrics for threshold design.
- SOC Equivalent:
  - Dashboards
  - Flow analytics
  - Traffic baselining

---

### `sudo -v`
- Refreshes sudo authentication timestamp.
- Forces authentication log generation.
- SOC Use:
  - Validate privileged command execution
  - Track user actions

---

### `journalctl | grep sudo | tail`
- Filters system logs for sudo events.
- Confirms endpoint telemetry presence.
- SOC Use:
  - Incident investigation
  - Privilege auditing

---

### `journalctl --since "10 minutes ago"`
- Extracts logs for a defined time window.
- Preserves evidence for analysis.
- SOC Use:
  - Timeline reconstruction
  - Forensic triage

---

## Detection Signal Development

Observed Signal:
- High volume of TCP SYN packets from a single source.
- Multiple destination ports targeted.
- Short time burst behavior.

Example Detection Logic:
- If a host sends >1000 SYN packets within 2 minutes
across >50 destination ports, trigger alert.


---

## False Positive Considerations

Potential benign sources:
- Vulnerability scanners
- Monitoring systems
- Load balancers
- Authorized penetration testing

Detections must be tuned to reduce unnecessary alerts.

---

## Key Learnings

- Detection engineering focuses on behavioral signals.
- Telemetry must be measurable and repeatable.
- Metrics enable threshold-driven detection.
- Logs and network data complement each other.
- Correlation improves investigation accuracy.
- Documentation strengthens operational maturity.

---
