# TCP 3-Way Handshake Analysis

## Objective
To capture and analyze the TCP 3-way handshake using Wireshark and understand how a TCP connection is established at the network level.

---

## Environment
- OS: Kali Linux
- Network Interface: eth0
- IP Address: 192.168.15.129
- Tool: Wireshark
- Traffic Generator: curl

---

## Activity Performed
1. Identified the active network interface using `ip a`.
2. Started packet capture on interface `eth0` in Wireshark.
3. Generated TCP traffic by running:
4. Applied Wireshark filters to observe TCP handshake packets.
5. Captured and saved the traffic for analysis.

---

## TCP 3-Way Handshake Explanation
A TCP connection is established using three steps:

1. **SYN (Synchronize)**  
- Client sends a SYN packet to initiate the connection.

2. **SYN-ACK (Synchronize + Acknowledge)**  
- Server acknowledges the SYN and sends its own SYN.

3. **ACK (Acknowledge)**  
- Client acknowledges the server response.

After this exchange, the TCP connection is successfully established and data transfer begins.

---

## Observations
- TCP handshake packets were observed in the following order:
- SYN
- SYN-ACK
- ACK
- The handshake completed before any application-layer (HTTP) data was transmitted.
- Source IP: 192.168.15.129
- Destination IP: example.com server IP

---

## Detection Perspective
From a security and monitoring standpoint:
- Normal TCP connections complete the full 3-way handshake.
- Multiple SYN packets without corresponding ACKs may indicate:
- Port scanning activity
- SYN flood or denial-of-service attempts
- Blue Teams monitor incomplete or abnormal handshakes to detect malicious behavior.

---

## Evidence
- Packet Capture:
- `tcp-handshake-capture.pcapng` (stored in `07-Evidences/Packet-Captures/`)
- Screenshot (optional):
- Highlighted SYN, SYN-ACK, and ACK packets in Wireshark

---

## Key Learnings
- Understood how TCP establishes reliable connections.
- Learned the meaning and role of SYN and ACK flags.
- Observed how TCP handshake appears in packet captures.
- Built foundational knowledge useful for both attack detection and network defense.
