# Port Scan Detection Rule Engineering

## Objective
To translate observed network scanning behavior into structured detection logic that can be operationalized in a SIEM or detection platform.

This lab focuses on:
- Converting packet-level observations into detection indicators
- Designing thresholds and logical conditions
- Understanding false positives and tuning considerations
- Building detection-engineering mindset for SOC environments

---

## Environment
- OS: Kali Linux (VMware)
- Network Interface: eth0
- Tools:
  - Wireshark
  - Nmap
  - Text-based rule modeling
- Data Sources:
  - Baseline traffic packet capture
  - Port scan traffic packet capture

---

## Observed Behavior

### Normal Baseline Traffic
- Few TCP SYN packets observed.
- TCP handshakes completed normally.
- Limited destination ports contacted.
- Low and unstructured traffic volume.

### Port Scan Traffic
- High volume of TCP SYN packets in a short time window.
- Multiple destination ports targeted by a single source IP.
- Many connections did not complete TCP handshake.
- Structured and repetitive probing behavior.

---

## Detection Indicators

Primary indicators identified:

1. Elevated TCP SYN packet rate from a single source.
2. High destination port diversity within a short time window.
3. Low percentage of completed TCP handshakes.
4. Sequential or patterned port probing behavior.
5. Repeated connection attempts without sustained sessions.

These indicators collectively differentiate reconnaissance activity from legitimate traffic.

---

## Detection Logic (Human Readable)

If a single source IP sends TCP SYN packets to more than 10 unique destination ports
within a 60-second window and less than 30% of those connections complete a handshake,
then raise a potential network scanning alert.

---

## Example SIEM-Style Query (Conceptual)

### Splunk-style Logic Example

index=network
| where tcp_flags="SYN"
| stats dc(dest_port) as unique_ports by src_ip
| where unique_ports > 10

This demonstrates how packet metadata could be aggregated to identify scanning behavior.

---

## False Positive Considerations

Legitimate activities that may resemble scanning behavior:

- Vulnerability assessment tools
- Network inventory discovery
- Monitoring health checks
- Load balancer probing
- Authorized security testing

Detection rules must be tuned to reduce unnecessary alerts.

---

## Detection Improvement Ideas

- Correlate scanning activity with authentication logs.
- Track repeated scans from the same source across longer time windows.
- Whitelist trusted scanning systems.
- Combine SYN rate, port diversity, and handshake completion metrics.

---

## Evidence
  - `baseline-normal-traffic-v2.pcapng` and `scan-traffic.pcapng` (stored in `07 -    Evidences/Packet-captures/`)

---

## Key Learnings

- Detection rules are logical representations of attacker behavior.
- Threshold selection directly impacts alert quality.
- False positives must always be evaluated.
- Detection engineering bridges offensive and defensive operations.
- Structured logic enables scalable monitoring.