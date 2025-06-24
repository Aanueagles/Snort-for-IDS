# 🛡️ Snort Intrusion Detection System (IDS) Lab

This repository documents the full setup, configuration, and testing of Snort — a powerful open-source Intrusion Detection System — in a virtual lab environment. This project was inspired by the YouTube video tutorial on Snort IDS practical deployment ([Watch here](https://youtu.be/uPdCmuFh40M)).

## 📌 Table of Contents

- [Lab Objectives](#lab-objectives)
- [Tools & Environment](#tools--environment)
- [Installation Steps](#installation-steps)
- [Configuration](#configuration)
- [Creating and Testing Rules](#creating-and-testing-rules)
- [Log Analysis](#log-analysis)
- [Screenshots](#screenshots)
- [Key Learnings](#key-learnings)
- [References](#references)

---

## 🎯 Lab Objectives

- Understand the role of Intrusion Detection Systems.
- Install and configure Snort on a Linux machine.
- Write and test custom Snort rules.
- Analyze network traffic and alert logs.
- Simulate basic attack traffic (ping, TCP scan, etc.).

---

## 🧪 Tools & Environment

| Tool | Version |
|------|---------|
| OS | Ubuntu 22.04 LTS |
| IDS | Snort (v2.x) |
| Packet Generator | nmap, ping |
| Traffic Analysis | Snorpy, Wireshark |
| Editor | nano / vim |
| Topology | Attacker VM ↔ Snort IDS ↔ Internet |

---

## ⚙️ Installation Steps

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install snort -y
```

During installation, select the monitoring interface (e.g., eth0).

Verify installation:

```bash
snort -V
```
Enable promiscuous mode:
```bash
sudo ip link set eth0 promisc on
```
|Replace eth0 with your actual network interface (use ip a to check).

🛠️ Configuration
-Main config file: /etc/snort/snort.conf
-Set your local network:
```conf
var HOME_NET 192.168.1.0/24
```
|Replace 192.168.1.4  with your actual network address (use ip a to check).

-Ensure local.rules is included:
```conf
include $RULE_PATH/local.rules
```
-Enable output plugin:
```conf
output alert_fast: stdout
```

✍️ Creating and Testing Rules
Sample ICMP Detection Rule
```conf
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping Detected"; sid:1000001; rev:1;)
```
Sample TCP SYN Scan Rule
```conf
alert tcp any any -> $HOME_NET any (flags:S; msg:"TCP Scan Detected"; sid:1000002; rev:1;)
```
-Test rule syntax:
```bash
sudo snort -T -c /etc/snort/snort.conf
```
-Run Snort in IDS mode:
  ```bash
  sudo snort -q -A console -c /etc/snort/snort.conf -i eth0
  ```
  | Replace eth0 with your actual network interface (use ip a to check).
  
Or run Snort on a PCAP file:
```bash
sudo snort -A console -q -c /etc/snort/snort.conf -r /path/to/file.pcap
```
🧪 Simulating Attacks
1. ICMP Test:
   ```bash
   ping <Snort_IP>
   ```
2. TCP SYN Scan Test:
   ```bash
   nmap -sS <Snort_IP>
   ```
    |Observe alerts in the terminal or /var/log/snort/alert.

📂 Log Analysis
Default alert path: /var/log/snort/alert
Use cat or less to view logs
CSV or unified2 output plugins can be enabled for advanced SIEM tools

📸 Screenshots
(Upload images in /screenshots folder)
✅ Snort installation success
✅ Config test passed
✅ Console alerts
✅ Custom rules in action

📘 Key Learnings
Snort can operate in various modes (sniffer, packet logger, IDS).
Custom rules are defined using human-readable syntax.
IDS placement and interface setup is critical for visibility.
Alert logs can provide rich data for incident response.

🧠 Author
Oyedepo Joseph Aanuoluwapo
Cybersecurity Enthusiast | SOC Analyst | Blue Team Labs
RedeemedNet - Protecting the value within.
