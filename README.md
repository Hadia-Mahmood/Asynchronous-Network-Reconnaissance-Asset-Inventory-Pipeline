# Asynchronous Network Reconnaissance & Asset Inventory Pipeline

A high-performance network mapping and service discovery utility built with Python. This project combines a custom multithreaded network scanner (utilizing raw socket manipulation via Scapy) with an automated fingerprinting suite (utilizing python-nmap) to find alive hosts, identify open ports, extract banners, and compile structural asset inventories.

## 🛠️ System Architecture & Workflow

The framework operates in two distinct execution blocks:
1. **Asynchronous Host & Port Discovery Engine:**
   - **ICMP Ping Sweep:** Rapidly maps live nodes across a given subnet mask using concurrent ICMP echo requests.
   - **TCP SYN Port Scan:** Executes multithreaded stealth scanning across active ports (1–1023) by monitoring for `SYN-ACK` flags (`SA`) without completing the full three-way handshake.
2. **Service Mapping & Banner Enumerator:**
   - Target nodes pass into an automated service scanner to probe OS families, engine versions, and running daemon metadata.
   - Collected metrics are serialized on-the-fly into standardized CSV sheets.

## 🚀 Features

- **Asynchronous Execution Scaling:** ThreadPoolExecutor dynamically scales worker pools across native CPU core boundaries to eliminate synchronous network timeouts.
- **Thread-Safe State Logging:** Implements thread locking arrays (`threading.Lock`) to handle race conditions during real-time console status reporting.
- **Structured Asset Logging:** Exports critical vulnerability reconnaissance data seamlessly into standardized `.csv` reports for audit consumption.

## 📋 Prerequisites

### System Requirements
- **Root/Administrator Privileges:** Operating raw Layer 3 network sockets via Scapy (e.g., executing custom SYN flags) requires escalated system permissions.
- **Nmap Core binaries:** Your host system must have Nmap installed and appended to your environment pathing variables.

### Installation
```bash
# Install core dependencies
pip install scapy python-nmap
```

## 💻 Getting Started

### Step 1: Subnet Discovery (Ping Sweep & Stealth Port Scan)
Provide the target base network and subnet mask as command-line arguments to run the custom Scapy engine:

```bash
sudo python scanner.py 192.168.1.0 255.255.255.0
```

### Step 2: Target Banner Grabbing & Asset Exporting
Pass a specific host or target range, define targeted ports, and indicate an export name to extract application versions:

```bash
python banner_grabber.py 192.168.1.1 -p 22,80,443 -o infrastructure_inventory.csv
```

## 📊 Sample Asset Output

### Console Manifestation
```text
Scanning IP: 192.168.1.1
Scanning ports: 22,80,443

Scan results:
IP: 192.168.1.1
OS: Linux
Port: 22
Name: ssh
Product: OpenSSH
Version: 8.9p1 Ubuntu 3ubuntu0.1
```

### Generated Schema File (`scan_results.csv`)
```csv
ip,os,port,name,product,version
192.168.1.1,Linux,22,ssh,OpenSSH,8.9p1 Ubuntu 3ubuntu0.1
192.168.1.1,Linux,443,http,nginx,1.18.0
```

## ⚠️ Disclaimer
This script is designed strictly for local infrastructure audits, academic research, and authorized penetration testing exercises. Ensure you possess explicit written authorization before evaluating external networks.
