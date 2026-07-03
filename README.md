# Network Intrusion Detection System (IDS) Lab using Suricata

## Overview
A hands-on cybersecurity lab simulating real-world network attacks 
and detecting them using Suricata, an open-source high-performance 
Network Intrusion Detection System (IDS). This project demonstrates 
practical understanding of network security monitoring, threat 
detection, and custom rule engineering.

## Project Goals
- Build an isolated virtual lab environment for safe attack simulation
- Deploy and configure Suricata IDS with 51,000+ detection rules
- Simulate real network attacks (port scanning, brute force)
- Write custom detection rules to identify specific attack patterns
- Analyze and document attack signatures in IDS logs

## Lab Architecture

┌─────────────────┐         ┌──────────────────────┐
│   Attacker      │ ──────► │   Target + IDS       │
│   WSL2 Ubuntu   │         │   Kali Linux         │
│ 192.168.100.86  │         │ 192.168.100.104      │
│                 │         │ Suricata 8.0.5        │
│ Tools:          │         │ 51,182 rules loaded  │
│ - Nmap 7.98     │         │                      │
│ - Hydra 9.6     │         │ Logs:                │
└─────────────────┘         │ - fast.log           │
│ - eve.json           │
└──────────────────────┘
Isolated VMware Host-Only Network

## Tools & Technologies
| Tool | Version | Purpose |
|------|---------|---------|
| Suricata | 8.0.5 | Network IDS engine |
| Nmap | 7.98 | Port scan simulation |
| Hydra | 9.6 | SSH brute force simulation |
| WSL2 Ubuntu | 24.04 | Attacker machine |
| Kali Linux | 2026 | Target + IDS sensor |
| Emerging Threats | Latest | Community rule set |

## Attacks Simulated & Detected

| Attack Type | Tool | Suricata Alert | MITRE ATT&CK | Detected |
|-------------|------|----------------|--------------|----------|
| SYN Stealth Port Scan | Nmap -sS | POSSBL PORT SCAN (NMAP -sS) | T1046 - Network Service Discovery | ✅ Yes |
| SSH Brute Force | Hydra | Custom Rule - SSH Brute Force Attempt | T1110 - Brute Force | ✅ Yes |
| Suspicious DNS Lookup | RustDesk | ET INFO RustDesk Domain in DNS Lookup | T1071 - Application Layer Protocol | ✅ Yes |

## Custom Detection Rule
Written in `/etc/suricata/rules/local.rules`:
alert tcp any any -> $HOME_NET 22 (msg:"SSH Brute Force Attempt
Detected"; flow:to_server; threshold:type threshold, track by_src,
count 5, seconds 60; sid:9000001; rev:1;)

**Rule Explanation:**
- `alert tcp` — monitor TCP traffic
- `any any -> $HOME_NET 22` — from anywhere to SSH port 22
- `msg` — alert message shown in logs
- `threshold: count 5, seconds 60` — trigger if 5+ attempts in 60 seconds
- `sid:9000001` — unique rule ID

## Key Findings
- Suricata detected Nmap SYN scan within seconds of attack initiation
- 51,182 rules loaded from Emerging Threats open ruleset
- Custom SSH brute force rule successfully written and tested
- Hydra successfully cracked weak password (`kali:kali`) in under 10 seconds — demonstrating why strong passwords matter
- Loopback traffic (127.0.0.1) not inspected by Suricata — identified as architectural limitation of host-based IDS deployment
- RustDesk remote desktop software flagged automatically by ET rules — demonstrating real-world applicability of signature-based detection

## MITRE ATT&CK Mapping
Reconnaissance → T1046 (Network Service Discovery) — Nmap scan
Credential Access → T1110 (Brute Force) — Hydra SSH attack
Command & Control → T1071 (Application Layer Protocol) — DNS anomaly

## Limitations & Future Improvements
| Limitation | Future Improvement |
|------------|-------------------|
| Loopback traffic not monitored | Deploy dedicated IDS sensor with port mirroring |
| Single network segment | Add multiple VLANs to simulate enterprise network |
| No SIEM integration | Connect Suricata to ELK Stack or Wazuh SIEM |
| Manual rule writing | Implement automated rule generation from threat feeds |
| No IPS mode | Enable Suricata in IPS mode to actively block attacks |

## What I Learned
- How to deploy and configure Suricata IDS from scratch
- How network attacks appear at the packet and log level
- How to write custom Suricata detection rules using threshold logic
- How Nmap SYN stealth scans differ from regular TCP scans
- How brute force attacks generate patterns detectable by IDS
- Practical understanding of MITRE ATT&CK framework mapping
- Limitations of host-based vs network-based IDS deployment

## Part of a Larger Portfolio
This is **Project 1 of 9** in my Network Security Lab Series, built 
as part of preparing for graduate study in Network Security and 
Cybersecurity. Each project builds on skills from the previous one.

**Full Series:**
1. ✅ Network IDS Lab (Suricata) — This project
2. 🔄 Manual Traffic Analysis & Anomaly Detection
3. ⏳ Man-in-the-Middle Attack & Defense Lab
4. ⏳ Wireless Network Security Audit
5. ⏳ DNS Security Lab (Spoofing + DNSSEC)
6. ⏳ VPN & Tunnel Security Lab
7. ⏳ Network Segmentation & Zero Trust Lab
8. ⏳ SDN Automated Firewall
9. ⏳ Full Penetration Testing Report (Capstone)

## References
- [Suricata Documentation](https://suricata.io/documentation/)
- [Emerging Threats Rules](https://rules.emergingthreats.net/)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [Nmap Reference Guide](https://nmap.org/book/man.html)

---
*Built by Anam Batool | Software Engineering Graduate | 
Virtual University of Pakistan*
*Focus: Network Security & Cybersecurity | 


