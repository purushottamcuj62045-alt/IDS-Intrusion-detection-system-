# Network Intrusion Detection System (IDS)

A real-time Network Intrusion Detection System built in Python using Scapy and Machine Learning (Isolation Forest) to detect SYN floods, port scans, and anomalous network traffic.

---

## Features

- **Live Packet Capture** — captures real-time TCP/UDP traffic using Scapy
- **Signature-based Detection** — detects known attack patterns like SYN floods and port scans
- **ML Anomaly Detection** — uses Isolation Forest to flag unusual traffic patterns
- **Structured Alerts** — logs JSON-formatted alerts with timestamps and confidence scores
- **Multi-threaded** — non-blocking packet capture using Python threading and queues

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3 | Core language |
| Scapy | Packet capture and analysis |
| Scikit-learn | Isolation Forest anomaly detection |
| NumPy | Feature vector processing |
| Threading | Non-blocking packet queue |
| Logging | Structured JSON alert logging |

---

## Project Structure

```
IDS/
├── ids.py          # Main IDS code (all classes)
├── test_ids.py     # Test suite with simulated attacks
├── ids_alerts.log  # Generated alert log file
└── README.md       # Project documentation
```

---

## Installation

**Clone the repository:**
```bash
git clone https://github.com/yourusername/ids.git
cd ids
```

**Install dependencies:**
```bash
pip install scapy scikit-learn numpy --break-system-packages
```

Or using apt on Kali Linux:
```bash
sudo apt install python3-scapy python3-numpy python3-sklearn -y
pip install python-nmap --break-system-packages
```

---

## Usage

**Run the IDS (requires root for packet capture):**
```bash
sudo python3 ids.py
```

**Run on a specific interface:**
```python
ids = IntrusionDetectionSystem(interface="wlan0")  # change in ids.py
```

**Find your interface:**
```bash
ip a
```

**Run the test suite:**
```bash
sudo python3 test_ids.py
```

---

## How It Works

### 1. Packet Capture
Scapy sniffs live traffic on the specified network interface. Only TCP and UDP packets are processed; all others are ignored.

### 2. Traffic Analysis
Each packet is analyzed to extract flow statistics:
- `packet_size` — size of the packet in bytes
- `packet_rate` — packets per second for this flow
- `byte_rate` — bytes per second for this flow
- `flow_duration` — how long the flow has been active
- `tcp_flags` — TCP control flags (SYN, ACK, etc.)

### 3. Threat Detection

**Signature-based:**

| Rule | Condition |
|------|-----------|
| SYN Flood | TCP flag = SYN and packet rate > 100/sec |
| Port Scan | Packet size < 100 bytes and packet rate > 200/sec |

**Anomaly-based (Isolation Forest):**
- Trained on normal traffic patterns
- Flags traffic with anomaly score below `-0.5`
- Catches unknown attacks that signature rules miss

### 4. Alert System
Alerts are printed to the terminal and saved to `ids_alerts.log` in JSON format:
```json
{
  "timestamp": "2026-06-28T09:22:43.369385",
  "threat_type": "signature",
  "source_ip": "192.168.1.100",
  "destination_ip": "192.168.1.2",
  "confidence": 1.0,
  "details": {
    "type": "signature",
    "rule": "port_scan",
    "confidence": 1.0
  }
}
```

---

## Test Results

```
------------------------------------------------------------
                     IDS TEST SUITE
------------------------------------------------------------

[PASS] Normal HTTP Traffic
       Expected threat: False | Detected: False

[PASS] Normal HTTPS Traffic
       Expected threat: False | Detected: False

[PASS] SYN Flood Simulation
       Expected threat: True | Detected: True
       -> SIGNATURE: syn_flood (confidence: 1.00)

[PASS] Port Scan Simulation
       Expected threat: True | Detected: True
       -> SIGNATURE: port_scan (confidence: 1.00)

------------------------------------------------------------
  Results: 4 passed, 0 failed out of 4 tests
------------------------------------------------------------
```

---

## How Isolation Forest Works

Isolation Forest detects anomalies by isolating outliers using random decision trees:

- **Normal traffic** — needs many splits to isolate (long path length)
- **Anomalous traffic** — isolated quickly with few splits (short path length)

The algorithm scores each sample; scores below `-0.5` are flagged as anomalies. This allows the IDS to catch unknown attacks that signature rules would miss.

---

## Disclaimer

This tool is intended for educational purposes and authorized network monitoring only. Do not use on networks you do not own or have explicit permission to monitor. Unauthorized packet capture may be illegal in your jurisdiction.

---

## License

MIT License — feel free to use, modify, and distribute.

---

## Author

[Purushottam]
[LinkedIn]([https://linkedin.com/in/yourprofile](https://www.linkedin.com/in/purushottam-k-08a414383/?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app | [GitHub][https://github.com/yourusername](https://github.com/purushottamcuj62045-alt
