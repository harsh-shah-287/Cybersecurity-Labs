# Threat Hunting Queries – Reconnaissance Detection 

## Objective

To define investigative queries and analytical logic used to validate the threat hunting hypothesis and uncover suspicious reconnaissance behavior.

Queries are platform-agnostic and represent analytical intent.

---

## Data Sources

- Network telemetry:
  - `nmap-syn-scan-statistics.csv`
- Endpoint telemetry:
  - systemd journal logs

---

## Hunt Query 1 – Identify All SYN Traffic

**Purpose:**
Isolate initial connection attempts for analysis.

Pseudo Query: 
```SQL 
SELECT * FROM network_logs WHERE tcp_flag = SYN AND tcp_ack = false
```

Linux Simulation:

```bash
tcp.flags.syn == 1 && tcp.flags.ack == 0
```
**Expected Outcome:** Dataset of SYN-only traffic for behavioral analysis.

## Hunt Query 2 - Source IP Frequency Analysis

**Purpose:** Identify hosts generating unusually high SYN volumes.

Pseudo Query:
```SQL
SELECT source_ip, COUNT(*) AS syn_count FROM network_logs GROUP BY source_ip ORDER BY syn_count DESC
```

Linux simulation:
```bash
tail -n +2 nmap-syn-scan-statistics.csv | cut -d',' -f1 | tr -d '"' | sort | uniq -c | sort -nr
```

**Expected Outcome:** Detection of dominant traffic generators.

---

## Hunt Query 3 – Destination Port Cardinality

**Purpose:**
Detect multi-port probing behavior.

Pseudo Query:
```SQL
SELECT source_ip, COUNT(DISTINCT destination_port) AS unique_ports FROM network_logs GROUP BY source_ip
```

Linux Simulation:
```bash
tail -n +2 nmap-syn-scan-statistics.csv | cut -d',' -f4 | sort | uniq | wc -l
```

**Expected Outcome:** Identification of abnormal port diversity

---

## Hunt Query 4 – Rate-Based Behavior

**Purpose:**
Detect burst or sustained scanning activity.

Pseudo Query:
```SQL
SELECT source_ip, COUNT(*) / window_time AS syn_rate FROM network_logs GROUP BY source_ip, 60-second window
```

**Manual Analysis:** Use timestamps in Wireshark to estimate rate and burst behavior.

**Expected Outcome:** Identification of abnormal packet velocity.

---

## Hunt Query 5 – Low-and-Slow Scan Detection

**Purpose:** Identify stealth scanning attempts.

Pseudo Query:

```SQL 
SELECT source_ip FROM network_logs GROUP BY source_ip, 10-minute window HAVING COUNT(*) > 50 AND COUNT(*) < 500
```

**Manual Analysis:** Look for persistent low-volume patterns over time.

**Expected Outcome:** Detection of stealth reconnaissance.

---

## Hunt Query 6 – Endpoint Privilege Correlation

**Purpose:**
Correlate network anomalies with endpoint activity.

Pseudo Query:
```SQL
SELECT * FROM network_events N JOIN endpoint_logs E ON N.source_ip = E.host_ip WHERE E.event_type = "sudo" AND time_delta <= 5 minutes
```

Linux Simulation:
```bash
sudo journalctl | grep sudo
```
**Expected Outcome:** Validate tool execution alignment.

---

## Hunt Query 7 – Error and Authentication Review

**Purpose:**
Detect potential unauthorized activity.

Linux Simulation:
```bash
sudo journalctl | grep -Ei "failed|invalid|authentication|error"
```

**Expected Outcome:** Identify suspicious login attempts or failures.

---

## Hunt Query 8 – Behavioral Sequence Reconstruction

**Purpose:** Reconstruct attacker workflow.

**Logic:** Privilege escalation → Network scan → Termination

**Method:** Correlate timestamps across telemetry sources.

**Expected Outcome:** Timeline validation.

---

## Analyst Notes

- Queries are iterative and exploratory.
- Results inform detection improvement.
- Hunting logic should evolve continuously.
- Platform syntax varies but logic remains consistent.
