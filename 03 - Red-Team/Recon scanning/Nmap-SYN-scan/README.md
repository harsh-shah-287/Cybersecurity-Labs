# Nmap SYN Scan Analysis

## Objective
To perform a TCP SYN scan using Nmap and analyze the generated network traffic to understand how automated port scanning appears at the packet level.

---

## Environment
- OS: Kali Linux (VMware)
- Network Interface: eth0
- IP Address: 192.168.15.129
- Target: Local Gateway (192.168.15.2)
- Tool: Nmap, Wireshark

---

## Activity Performed
1. Identified the local gateway using:
2. Started packet capture on interface `eth0` using Wireshark.
3. Executed a SYN scan:
sudo nmap -sS 192.168.15.2

4. Stopped capture after scan completion.
5. Applied filters to analyze TCP flags and responses.

---

## Observations
- Multiple TCP SYN packets were sent to various destination ports.
- The target responded with:
- RST packets for closed ports
- Possible SYN-ACK packets for open ports
- TCP handshakes were not completed by the scanner.

Observed traffic pattern:


SYN → RST
SYN → SYN-ACK (open ports)


---

## Detection Perspective
A SYN scan generates high volumes of SYN packets without full session establishment. Detection systems monitor:
- Rapid SYN bursts
- Multiple destination ports
- Incomplete handshakes

This behavior is commonly associated with reconnaissance activity.

**MITRE ATT&CK Mapping:**


T1046 – Network Service Scanning


---

## Evidence
- Packet Capture:
  - `nmap-syn-scan.pcapng` (stored in `07-Evidences/Packet-Captures/`)
- Screenshot (optional):
  - SYN and RST packets visible in Wireshark

---

## Key Learnings
- Understood how Nmap performs stealth SYN scanning.
- Learned to differentiate open and closed ports via packet responses.
- Gained experience correlating automated scans with packet-level evidence.