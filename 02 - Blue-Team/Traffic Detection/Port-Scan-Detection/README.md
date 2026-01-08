# Port Scan Detection – Baseline vs Scan Traffic

## Objective
To compare normal network traffic with port scanning traffic and identify indicators that can be used for detection.

---

## Environment
- OS: Kali Linux (VMware)
- Network Interface: eth0
- Baseline Capture: baseline-normal-traffic-v2.pcapng
- Scan Capture: scan-traffic.pcapng
- Tool: Wireshark, Nmap

---

## Baseline Traffic Observations
- Normal TCP sessions completed successfully.
- Few destination ports were contacted.
- TCP handshakes completed normally.
- Packet patterns were consistent with web browsing and DNS activity.

---

## Scan Traffic Observations
- High number of TCP SYN packets in a short time window.
- Multiple destination ports targeted by a single source.
- Many connections did not complete the TCP handshake.
- RST responses observed from closed ports.

---

## Detection Indicators
- Elevated SYN packet volume from a single source.
- High destination port diversity.
- Incomplete TCP handshakes.
- Repetitive structured traffic patterns.

---

## Example Detection Logic
Alert when a single host sends TCP SYN packets to more than 10 unique destination ports within 60 seconds without completing handshakes.

---

## False Positives Considerations
- Vulnerability scanners
- Network monitoring tools
- Legitimate inventory scans

---

## Evidence
- Packet Capture :
  - `baseline-normal-traffic-v2.pcapng` and `scan-traffic.pcapng` (stored in `07 - Evidences/Packet-captures/`)

---

## Key Learnings
- Learned how to establish a baseline for normal traffic.
- Understood behavioral differences between legitimate traffic and scanning activity.
- Practiced translating traffic observations into detection logic.
