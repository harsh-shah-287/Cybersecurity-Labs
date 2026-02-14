# Detection Architecture Overview

## Detection Layers

Layer 1 – Network Telemetry
- SYN thresholds
- Port diversity

Layer 2 – Endpoint Telemetry (journalctl)
- sudo usage

Layer 3 – Kernel Telemetry (auditd)
- execve monitoring
- file watch rules

Layer 4 – Correlation Engine
- Time window logic
- Multi-signal scoring

## Detection Philosophy

Single signal = medium confidence  
Correlated signals = high confidence  
Kernel telemetry confirmation = critical confidence  

This layered model improves resilience against evasion.
