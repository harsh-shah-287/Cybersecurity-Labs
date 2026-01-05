# DNS Traffic Analysis – UDP vs TCP

## Objective
To capture and analyze DNS query traffic using Wireshark and understand the difference between UDP and TCP in network communication.

---

## Environment
- OS: Kali Linux
- Network Interface: eth0
- IP Address: 192.168.15.129
- Tool: Wireshark
- Command Used: nslookup

---

## Activity Performed
1. Identified the active network interface using `ip a`.
2. Started packet capture on interface `eth0` using Wireshark.
3. Generated DNS traffic by running:
4. Applied Wireshark filters to isolate DNS packets.
5. Observed DNS query and response traffic.

---

## Observations
- DNS queries were sent over **UDP**.
- Destination port used: **53**.
- A DNS query was followed by a corresponding DNS response.
- No connection setup (handshake) was observed before data transfer.

---

## TCP vs UDP Comparison
- **TCP**
- Connection-oriented
- Reliable and stateful
- Uses a 3-way handshake
- **UDP**
- Connectionless
- Faster, lower overhead
- No handshake or session tracking

DNS primarily uses UDP to reduce latency and overhead for simple request–response queries.

---

## Detection Perspective
From a security monitoring standpoint:
- High volumes of DNS queries may indicate:
- Malware beaconing
- DNS tunneling
- DNS traffic is often monitored to detect suspicious domain lookups or abnormal query patterns.

---

## Evidence
- Packet Capture:
- `dns-query-capture.pcapng` (stored in `07-Evidences/Packet-Captures/`)
- Screenshot (optional):
- DNS query and response visible in Wireshark

---

## Key Learnings
- Understood why DNS uses UDP instead of TCP by default.
- Learned how to capture and filter DNS traffic in Wireshark.
- Built foundational understanding of protocol behavior from a detection perspective.
