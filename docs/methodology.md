# Lab Methodology

## Objective
Simulate real network attacks and detect them using Suricata IDS.

## Environment
- Attacker: WSL2 Ubuntu (192.168.100.86)
- Target/IDS: Kali Linux (192.168.100.104)

## Tools Used
- Suricata 8.0.5
- Nmap 7.98
- Hydra 9.6

## Attacks Performed
1. Nmap SYN Stealth Scan
2. SSH Brute Force (Hydra)

## Results
Both attacks successfully detected by Suricata.
