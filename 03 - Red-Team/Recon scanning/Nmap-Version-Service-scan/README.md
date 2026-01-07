# Nmap Version and Service Detection Analysis

## Objective
To perform service and version detection using Nmap and analyze the resulting network traffic to understand how enumeration activity differs from basic port scanning.

---

## Environment
- OS: Kali Linux (VMware)
- Network Interface: eth0
- IP Address: 192.168.15.129
- Target: Local Gateway (192.168.15.2)
- Tool: Nmap, Wireshark

---

## Activity Performed
1. Started packet capture on interface `eth0`.
2. Executed a combined scan:
sudo nmap -sS -sV 192.168.15.2
3. Observed service probes and responses.
4. Stopped packet capture and analyzed traffic patterns.

---

## Observations
- In addition to SYN packets, the scan generated:
- Service probe packets
- Application-layer responses (banners or resets)
- Increased packet volume compared to basic SYN scanning.
- Longer scan duration due to service fingerprinting.

---

## Detection Perspective
Version and service detection creates more distinguishable traffic patterns:
- Repeated probes to identified open ports
- Application-level fingerprinting traffic
- Elevated session attempts

Detection systems can identify:
- Enumeration behavior
- Excessive probing activity
- Abnormal service interactions

**MITRE ATT&CK Mapping:**
T1046 – Network Service Scanning
T1595 – Active Scanning


---

## Evidence
- Packet Capture:
  - `nmap-version-scan.pcapng` (stored in `07-Evidences/Packet-Captures/`)
- Screenshot (optional):
  - Service probe traffic in Wireshark

---

## Key Learnings
- Learned how service enumeration differs from port scanning.
- Observed how Nmap fingerprints services at the packet level.
- Developed understanding of detection implications for enumeration traffic.