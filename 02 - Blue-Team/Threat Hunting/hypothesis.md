# Threat Hunting Hypothesis – Reconnaissance Detection

## Purpose

To define a clear, testable hypothesis that guides proactive threat hunting activity using available telemetry.

A well-defined hypothesis ensures focused analysis and measurable outcomes.

---

## Primary Hypothesis

An endpoint may perform network reconnaissance activity using SYN-based scanning techniques that can evade basic threshold-based detections but can be identified through behavioral analysis and correlation with endpoint telemetry.


---

## Threat Context

Reconnaissance is commonly used by attackers to:

- Identify exposed services
- Map network surface area
- Discover vulnerable applications
- Prepare lateral movement and exploitation

Attackers often attempt to reduce scan visibility using:
- Low-rate scanning
- Distributed scanning
- Randomized timing
- Port subset probing

---

## MITRE ATT&CK Mapping

| Field | Value |
|------------|-----------------------------|
| Tactic | Discovery |
| Technique | **T1046 – Network Service Discovery** |
| Sub-techniques | SYN scanning, service enumeration |

---

## Expected Observable Behaviors

If the hypothesis is true, the following signals may appear:

### Network Indicators
- Elevated SYN packet counts relative to baseline
- Increased destination port diversity
- Incomplete TCP handshakes
- Repeated connection attempts to closed ports
- Temporal clustering of SYN traffic

### Endpoint Indicators
- Privileged command execution
- Network scanning tool usage
- Elevated process execution frequency
- Command-line activity aligned with scan timing

---

## Data Sources Available

- Network telemetry:
  - PCAP captures
  - TCP conversation statistics
- Endpoint telemetry:
  - systemd journal logs
  - sudo authentication logs

---

## Success Criteria

The hypothesis will be considered validated if:

- Behavioral scan patterns are observed in network telemetry.
- Endpoint activity correlates with network events.
- Patterns exceed expected baseline behavior.

The hypothesis will be considered disproven if:

- No anomalous patterns exist.
- Observed activity aligns fully with baseline behavior.

---

## Assumptions and Constraints

- Dataset represents a controlled lab environment.
- No external attacker traffic expected.
- Limited endpoint telemetry available.
- Detection logic is currently threshold-based.

---

## Analyst Notes

- Hypothesis may evolve as new telemetry becomes available.
- Future hunts should incorporate stealth behavior testing.
- Hypothesis-driven hunting improves analytical discipline.

---

