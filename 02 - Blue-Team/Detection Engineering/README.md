# Detection Engineering Fundamentals and Telemetry Validation Lab

## Objective
To understand the foundations of detection engineering and validate real telemetry generation by building an initial structured detection rule using Sigma format while mapping attacker behavior to the MITRE ATT&CK framework.

This lab focuses on:
- Detection engineering mindset  
- SOC telemetry understanding  
- Behavioral detection logic  
- Sigma rule structure  
- MITRE technique mapping  
- Signal vs noise awareness  
- Metrics-driven detection thinking  

---

## Environment
- OS: Kali Linux (VMware)  
- Network Interface: eth0  
- Tools:
  - Nmap  
  - Wireshark  
  - journalctl  
  - grep  
  - Git  
  - Text editor  
- Data Reference:
  - Network traffic captures from previous labs  
  - Log analysis artifacts  
  - Day 10 packet capture and log exports  

---

## Lab Structure

- Detection-Engineering/
- ├── README.md
- ├── notes.md
- ├── mitre-mapping.md
- └── sigma-rules/
- └── nmap-scan-detection.yml

---

## Detection Objective

- Detect potential port scanning activity based on abnormal TCP SYN behavior originating from a single source IP across multiple destination ports and short time windows.

---

## Telemetry Generated

### Network Telemetry
- Scan Type: TCP SYN Scan (`nmap -sS`)  
- Target: Local lab network  
- Total TCP SYN packets captured: **2811**  
- PCAP File: `nmap-syn-scan-capture-v2.pcapng`

### Endpoint Telemetry
- Sudo authentication events generated using `sudo -v`  
- Log export: `log-window.log`

Telemetry was validated using Wireshark filters and journal log review.

---

## Sigma Rule Created

- File: `nmap-scan-detection.yml`


Purpose:
- Demonstrate Sigma rule structure.  
- Represent scanning behavior in platform-neutral format.  
- Establish baseline detection documentation.  

This rule is currently conceptual and not yet deployed to a SIEM.

---

## MITRE Mapping

- Tactic: Discovery  
- Technique: T1046 – Network Service Discovery  

Reason:
- Port scanning is used to identify open services and reachable systems in a target environment.

---

## Key Concepts Practiced

- Indicator-based vs behavior-based detection  
- Telemetry validation and signal generation  
- Detection lifecycle understanding  
- Metrics extraction and threshold thinking  
- Signal vs noise trade-offs  
- False positive awareness  
- Detection documentation discipline  

---

## Learning Outcomes

By completing this lab, the following skills were developed:

- Ability to translate observed behavior into measurable detection signals.  
- Understanding of Sigma rule structure and purpose.  
- Familiarity with MITRE ATT&CK mapping.  
- Practical experience generating and validating telemetry.  
- Awareness of detection quality and tuning considerations.  
- Structured documentation of detection artifacts.  

---

## Future Enhancements

- Add threshold-based logic to the Sigma rule.  
- Convert Sigma rule into SIEM-native queries.  
- Validate detection using replayed traffic datasets.  
- Correlate detection with endpoint and authentication logs.  
- Expand detection coverage to additional techniques.  

---

## Evidence

- Detection Artifacts:
- `sigma-rules/nmap-scan-detection.yml`  
- `mitre-mapping.md`  
- `notes.md`  

- Network Evidence:
    - `nmap-syn-scan-capture-v2.pcapng`

- Log Evidence:
    - `log.window.log`