# Aging-Portrait Defense (APD)

APD is a Go based, bare metal security architecture that detects stealthy “low and slow” bit rot ransomware. It uses an in memory decoy fabric (ShadowCanvas) and dual metric drift analysis (Shannon entropy + Hamming distance) to catch corruption at a 0.005% threshold. When anomalies arise, APD executes an instant Inverse XOR Patch in memory, neutralizing decay before it touches production storage.
“The canvas bears the burden of the age, while the system remains untouched.”

# Purpose of this Repository
This repository documents Aging-Portrait Defense (APD) — a conceptual cybersecurity architecture designed to intercept and neutralize stealthy ransomware attacks before they impact production storage.
It is documentation only. No runnable source code is included. Instead, it provides:
1) The full academic report 
2) The conceptual design of APD’s architecture
3) Diagrams and visuals explaining the telemetry pipeline and defense engines
4) Pseudocode harness for the attack simulator (illustrating logic without exposing implementation)
5) Final results and telemetry outputs from controlled experiments

----------------------------------------------------------------------
# Architecture
1.	Capture Engine
    *	Out of band PCAP stream analysis
    *	Tracks Inter Arrival Time (IAT) variance and micro payload bit shifts
    *	Operates entirely in memory (no disk I/O overhead)
2.	ShadowCanvas Engine
    * Deploys isolated 4KB decoy blocks (decoy_db_01)
    *	Absorbs silent write attempts away from production disks
    *	Maintains pristine baselines for comparison
3.	Detection & Integrity Engine
    *	Shannon Entropy Variance (ΔH): Detects subtle randomness shifts
    *	Hamming Distance Drift: Enforces strict 0.005% threshold
    *	Filters false positives by correlating timing + content mutations
4.	Active Defense & Mitigation Tier
    *	Generates instant Inverse XOR Patch in memory
    *	Executes surgical rollback on affected blocks
    *	Freezes snapshot retention policies to prevent propagation
    *	Streams real time telemetry to SOC dashboards
--------------------------------------------------------
# Experimental Harness (Pseudocode)
APD includes a synthetic attack simulator to validate resilience against slow drip ransomware vectors. This harness generates micro payloads with entropy changes and enforces temporal delays to mimic stealthy attack pacing.
python

    # APD Attack Simulator & Bit-Rot Harness
    initialize_socket(target_ip="0.0.0.0", port=0000)

    for iteration in range(50):
    # Generate micro-payload with simulated entropy changes
    payload = generate_random_bytes(length=16)
    
    # Transmit stream into the APD capture interface
    send_packet(target_ip, target_port, payload)
    
    # Enforce slow-drip temporal delay to evade throughput alerts
    sleep(0.2)
-----------------------------------------------------------
# Live Telemetry Example
plaintext
2026/09/10 15:46:34 [APD ENGINE] Starting Aging-Portrait Defense Orchestrator...
2026/09/10 15:46:34 [SHADOW CANVAS] Initialized 3 decoy blocks in memory.
[15:46:40.859] SEVERITY: CRITICAL | Block: decoy_db_01
    ├── Bit-Drift Ratio : 0.210571% [████████████████░░░░]
    ├── Shannon ΔH      : 0.0004
    └── Auto-Mitigation : [INVERSE XOR PATCH READY (4096 byte)]
    
# References
1.	Advanced Threat Research Group, Stealthy Low-and-Slow Ransomware, 2025
2.	Enterprise Security Metrics Consortium, Limitations of EDR/SIEM in Detecting Micro-Anomalies, 2024
3.	IEEE Transactions on Information Forensics, Dual-Metric Drift Analysis, 2025
4.	Systems Engineering Proceedings, In-Memory Decoy Fabrics, 2024
5.	Resilient Systems Journal, Inverse XOR Patching for Zero-Disk Recovery, 2025
6.	Oscar Wilde, The Picture of Dorian Gray, 1890
-------------------------------------------------------
# Connect with Me
For collaborations, questions, or deeper technical discussions about APD, you can reach me here:
•	🌐 LinkedIn → www.linkedin.com/in/yara-al-otom 
•	📂 GitHub Portfolio → https://github.com/Yara-Alotom
