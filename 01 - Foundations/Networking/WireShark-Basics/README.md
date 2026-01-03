# Wireshark Basics – ICMP Packet Capture

## Objective
To capture and observe basic network packets using Wireshark and understand how ICMP traffic appears on the network.

---

## Environment
- OS: Kali Linux
- Network Interface: eth0
- IP Address: 192.168.15.129
- Tool: Wireshark

---

## Activity Performed
1. Identified the active network interface using `ip a`.
2. Started packet capture on interface `eth0` in Wireshark.
3. Generated ICMP traffic by running:
4. Observed ICMP Echo Request and Echo Reply packets.
5. Stopped the capture and saved it as a `.pcapng` file.

---

## Observations
- ICMP protocol was used for the ping operation.
- Each ping generated:
- One ICMP Echo Request (from local host)
- One ICMP Echo Reply (from destination)
- Source IP: 192.168.15.129
- Destination IP: Google public IP address

---

## Detection Perspective
ICMP traffic can be monitored in network environments to:
- Verify network reachability
- Detect reconnaissance or scanning behavior
- Identify abnormal ICMP usage patterns that may indicate network mapping

---

## Evidence
- Packet Capture:
- `icmp-ping-capture.pcapng` (stored in `07-Evidences/Packet-Captures/`)
- Screenshots (optional):
- Wireshark capture view showing ICMP packets

---

## Key Learnings
- Understood how to identify the correct network interface for packet capture.
- Learned basic Wireshark usage for capturing live traffic.
- Gained clarity on how ICMP packets appear on the network.
- Established a structured approach for documenting networking labs.
