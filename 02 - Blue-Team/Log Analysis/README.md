# Kali System Log Analysis and Event Correlation Lab

## Objective
To extract system and authentication logs from Kali Linux, analyze them for security-relevant activity, and correlate log events with captured network traffic and port scanning activity.

This lab builds foundational SOC skills in:
- Log acquisition and parsing
- Noise filtering and signal extraction
- Timeline reconstruction
- Cross-source correlation

---

## Environment
- OS: Kali Linux (VMware)
- Logging System: systemd journal
- Network Interface: eth0
- Timezone: EST
- Tools:
  - journalctl
  - grep
  - wc
  - Wireshark

---

## Log Sources Collected

| Log File | Description | Approx. Entries |
|------------|----------------|------------------|
| kali-journal.log | General journal snapshot | ~200 |
| kali-syslog.log | Exported system activity logs | ~500 |
| kali-auth.log | Authentication and privilege activity logs | ~856 |
| scan-time-window.log | Logs filtered to scan timeframe | Variable |

Logs were exported from the systemd journal since Kali does not generate traditional `/var/log/syslog` or `/var/log/auth.log` by default.

---

## Log Collection Commands

sudo journalctl -n 200 > kali-journal.log
sudo journalctl -n 500 > kali-syslog.log
sudo journalctl | grep -Ei "sudo|ssh|login|authentication" > kali-auth.log

Analysis Performed :

Authentication Activity Review
grep -Ei "sudo|ssh|login" kali-auth.log
Reviewed privileged command usage and login activity.

Activity aligned with legitimate administrative actions.

No suspicious authentication patterns detected.

System Errors and Warnings

grep -Ei "error|failed|warning" kali-syslog.log
Identified routine background warnings.

No security-impacting failures observed.

Network-Related Events

grep -Ei "network|eth0|dhcp|dns" kali-syslog.log
Observed DHCP lease renewals and interface activity.

Events aligned with packet capture timing.

Time-Based Correlation
Windows system operated in IST while Kali operated in EST.

Baseline capture time:

1:29 PM IST ≈ 02:59 AM EST

Logs were filtered using:
sudo journalctl --since "2026-01-08 02:45:00" --until "2026-01-08 03:15:00"

Correlated event identified:
sudo nmap -sS 192.168.15.2
This confirms privileged execution of the network scan corresponding to PCAP evidence.

## Timeline Summary
Time (EST)	Event
~02:53	Wireshark capture started
~02:58	DHCP lease renewal
~03:03	Nmap scan executed
~03:03	Packet capture stopped
~03:14	Network interface reconnected

## Detection Perspective
Logs validate attacker actions and timelines.
Correlation improves investigative accuracy.
Privileged command monitoring enhances visibility.
Multi-source telemetry strengthens detection confidence.

## Key Learnings
kali uses systemd journal logging instead of traditional syslog files.
Logs can be exported and analyzed using journalctl.
Grep enables efficient filtering of large log datasets.
Time normalization is critical for correlation.
Logs complement network-based detection.

# Evidence
- Logs :
  - `kali-journal.log`
  - `kali-syslog.log`, `kali-auth.log` and `scan-time-window.log` (stored in `02 - Blue-Team/Logs/`)


