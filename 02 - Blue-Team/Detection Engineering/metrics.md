# Detection Metrics – SYN Scan Telemetry 

## Capture Reference

- PCAP File: `mmap-syn-scan-capture-v2.pcapng`

- Capture Type: Controlled TCP SYN Scan
- Tool Used: Nmap (`-sS`)
- Interface: eth0
- Environment: Kali Linux (VMware)

---

## SYN Scan Metrics

| Metric | Value |
|--------|-------|
| Total TCP Conversations | **1686** |
| Total SYN Packets (A → B) | **1686** |
| Unique Destination Ports | **843** |
| Destination IPs | **192.169.15.2** |
| Scan Duration | **~50.52 seconds** |
| Average SYN Rate | **~33.37 packets/sec** |
| Traffic Pattern | Burst-based, highly structured |
| TCP Handshake Completion | Majority incomplete |

---

## Temporal Characteristics

- First observed SYN packet at ~43.57 seconds (relative capture time).
- Final SYN activity completed by ~94.09 seconds.
- Total active scanning window approximately **50 seconds**.
- Sustained high packet rate throughout scan execution.

---

## Behavioral Characteristics

Observed behavior is consistent with reconnaissance activity:

- Large number of SYN packets sent in a short time window.
- High destination port diversity (843 unique ports).
- Minimal bidirectional traffic.
- No sustained TCP sessions established.
- Structured and sequential probing behavior.

This behavior aligns with network service discovery patterns.

---

## Baseline Comparison (Conceptual)

Expected baseline behavior:

- Significantly lower SYN packet volume.
- Limited destination port diversity.
- Normal TCP handshakes complete.
- No burst traffic patterns.

Baseline PCAP should be captured separately for tuning.

---

## Detection Signal Definition

Primary detection signal:High volume of TCP SYN packets from a single source IP
targeting hundreds of destination ports within a short time window.

---

## Observed Traffic Characteristics

- High burst volume of SYN packets in a short time window.
- Multiple destination ports targeted sequentially.
- Majority of connections did not complete full TCP handshake.
- RST responses observed from closed ports.
- Traffic pattern was highly structured and repetitive.

This behavior is consistent with reconnaissance and service discovery activity.

---

Supporting indicators:
- Port diversity (843 ports)
- Sustained packet rate (~33 SYN/sec)
- Short execution window (~50 seconds)
- Incomplete handshakes

---

## Threshold Engineering Inputs

Initial threshold candidates derived from telemetry:

- SYN packet volume: 500 SYN Packets within 60 seconds.
- Destination port Diversity: 100 unique ports
- Sustained Packet Rate: 10 packets per sec.

---
## Operational Detection Metrics

To evaluate detection effectiveness in production environments:

| Metric | Description |
|--------|-------------|
| Alert Volume | Number of alerts generated per day |
| False Positive Rate | % of alerts closed as benign |
| Mean Time to Detection (MTTD) | Time from activity to alert |
| Mean Time to Response (MTTR) | Time from alert to resolution |
| Signal-to-Noise Ratio | True alerts vs total alerts |

---

## Analyst Notes

- Packet counter showed higher raw packet volume due to retransmissions.
- Conversation statistics represent normalized behavioral metrics.
- Metrics are suitable for threshold-based detection modeling.
- Correlation with endpoint telemetry increases detection confidence.
- Thresholds should be adjusted per environment.

---


