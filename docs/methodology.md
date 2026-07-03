# Methodology: Network Intrusion Detection System Lab

## 1. Problem Statement

Network intrusion detection is a critical component of modern 
cybersecurity infrastructure. As cyber attacks become increasingly 
sophisticated, organizations require automated systems capable of 
identifying malicious traffic in real time. Traditional perimeter 
defenses (firewalls) are insufficient alone — once an attacker 
bypasses the perimeter, lateral movement and data exfiltration 
can go undetected for months.

This project addresses the question:
**"Can a properly configured open-source IDS reliably detect 
common network attacks in a controlled lab environment?"**

According to IBM's Cost of a Data Breach Report, the average 
time to identify a breach is 197 days — demonstrating a critical 
need for effective intrusion detection systems.

## 2. Objectives
- Deploy Suricata IDS in a virtualized lab environment
- Simulate realistic network attacks against a target machine
- Evaluate Suricata's detection capability against each attack
- Develop a custom detection rule to extend default capabilities
- Identify architectural limitations of host-based IDS deployment

## 3. Lab Environment

### Network Topology
Attacker (WSL2 Ubuntu)  ──►  Target + IDS (Kali Linux)
192.168.100.86               192.168.100.104
Suricata 8.0.5

### Hardware
- Host Machine: Windows 11
- RAM: 8GB (shared between host and VMs)
- Virtualization: VMware Workstation + WSL2

### Software
| Component | Version |
|-----------|---------|
| Suricata IDS | 8.0.5 |
| Emerging Threats Ruleset | July 2026 |
| Kali Linux | 2026.1 |
| WSL2 Ubuntu | 24.04 LTS |
| Nmap | 7.98 |
| Hydra | 9.6 |

## 4. Methodology

### Phase 1 — Environment Setup
1. Installed Suricata 8.0.5 on Kali Linux
2. Configured Emerging Threats open ruleset (51,182 rules)
3. Configured WSL2 Ubuntu as isolated attacker machine
4. Verified network connectivity between attacker and target
5. Enabled real-time logging via fast.log and eve.json

### Phase 2 — Attack Simulation
Attacks were performed from attacker machine (192.168.100.86) 
targeting Kali Linux (192.168.100.104):

**Attack 1: Nmap SYN Stealth Scan**

Command: nmap -sS 192.168.100.104
Purpose: Simulate network reconnaissance
**Attack 2: SSH Brute Force**

Command: hydra -l kali -P passwords.txt 127.0.0.1 ssh -t 4
Purpose: Simulate credential-based attack
Result: Password 'kali' cracked in under 10 seconds

### Phase 3 — Custom Rule Development
A custom detection rule was written to identify SSH brute force 
patterns not covered by default rules:

alert tcp any any -> $HOME_NET 22 (msg:"SSH Brute Force Attempt
Detected"; flow:to_server; threshold:type threshold, track by_src,
count 5, seconds 60; sid:9000001; rev:1;)
### Phase 4 — Analysis & Documentation
- Reviewed fast.log and eve.json for generated alerts
- Mapped each attack to MITRE ATT&CK framework
- Identified and documented limitations

## 5. Results

### Detection Results
| Attack | Alerts Generated | Detection Time | Result |
|--------|-----------------|----------------|--------|
| Nmap SYN Scan | Multiple | ~3 seconds | ✅ Detected |
| SSH Brute Force | 1 (custom rule) | ~10 seconds | ✅ Detected |
| RustDesk DNS | Multiple | Immediate | ✅ Detected |

### Rules Performance
- Total rules loaded: **51,182**
- Rules from Emerging Threats: **50,808**
- Custom rules written: **1**
- False positives observed: **RustDesk legitimate software flagged**

### Sample Alert (Nmap Detection)
07/03/2026 [] [1:3400002:2] POSSBL PORT SCAN (NMAP -sS)
[] [Classification: Attempted Information Leak] [Priority: 2]
{TCP} 192.168.100.86 -> 192.168.100.104

The experience of writing a custom detection rule highlighted 
the importance of understanding attack patterns at the network 
level — a skill directly applicable to Security Operations Center 
(SOC) analyst and network security engineer roles.

