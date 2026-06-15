# 🐉 PenTest Dashboard

A web-based penetration testing dashboard for **Kali Linux on Raspberry Pi 4**.  
Wraps nmap, nikto, whois, dig, sslscan, whatweb, and curl in a clean real-time browser UI with saved reports.

---

## Quick Install

```bash
# 1. Transfer the zip to your Pi and extract it
unzip pentest-dashboard.zip
cd pentest-dashboard

# 2. Run the installer (installs tools, creates service, opens firewall)
sudo bash install.sh

# 3. Open in any browser on your network
http://<your-pi-ip>:5000
```

The installer handles everything: packages, Flask, systemd service, and firewall.

---

## Files in This Package

```
pentest-dashboard/
├── install.sh              ← Run this first (requires sudo)
├── uninstall.sh            ← Clean removal
├── update.sh               ← Update an existing install
├── app.py                  ← Flask application (backend)
├── requirements.txt        ← Python deps (flask only)
├── README.md               ← This file
├── templates/
│   ├── index.html          ← Scanner UI
│   ├── reports.html        ← Reports list page
│   ├── report_detail.html  ← Individual report viewer
│   └── report_export.html  ← Standalone HTML export template
├── reports/                ← Empty; reports saved here after install
└── static/                 ← Static assets (empty; CSS/JS inline)
```

After install, the app lives at `/opt/pentest-dashboard/`.  
Saved reports are stored at `/opt/pentest-dashboard/reports/*.json` and survive updates.

---

## Features

### Scanner (`/`)
- **15 scan types** across 5 categories
- Real-time streaming output in a terminal window
- Live syntax highlighting (open ports, IPs, CVEs, VULNERABLE)
- Target queue: add multiple IPs/domains, pick which to scan
- Target type auto-detection: IPv4, IPv6, CIDR, IP range, domain
- Scan history and bookmarks (saved in browser localStorage)

### Scan Types

| Category | Scan | Tool |
|---|---|---|
| Recon | Ping / Reachability | ping |
| Recon | WHOIS Lookup | whois |
| Recon | DNS Records | dig |
| Port Scanning | Quick (top 100 ports) | nmap |
| Port Scanning | Full (all 65535 ports) | nmap |
| Port Scanning | UDP Top 100 | nmap |
| Enumeration | Service & Version Detection | nmap |
| Enumeration | OS Detection | nmap |
| Enumeration | Firewall Detection | nmap |
| Web | HTTP Headers | curl |
| Web | HTTPS Headers | curl |
| Web | SSL/TLS Scan | sslscan |
| Web | Web Tech Fingerprint | whatweb |
| Vulnerability | Nmap Vuln Scripts | nmap |
| Vulnerability | Nikto Web Scan | nikto |

### Reports (`/reports`)
- Save any completed scan as a structured report
- Automatic risk scoring (0–100) with level: Low / Medium / High / Critical
- Risk flags: risky open ports, missing security headers, weak SSL, CVEs
- Per-report view with collapsible sections:
  - Risk flags, open ports table, vulnerabilities, OS guess, HTTP headers, SSL issues, DNS records
  - Raw terminal output with syntax highlighting
  - Analyst notes (editable per report)
- Export to **standalone HTML** or **plain text**
- Delete reports individually
- Filter by risk level on the reports list

---

## Service Management

```bash
# Start / stop / restart
sudo systemctl start   pentest-dashboard
sudo systemctl stop    pentest-dashboard
sudo systemctl restart pentest-dashboard

# Check status
sudo systemctl status  pentest-dashboard

# View live logs
sudo journalctl -u pentest-dashboard -f

# Disable auto-start on boot
sudo systemctl disable pentest-dashboard
```

---

## Updating

When a new version is available:

```bash
unzip pentest-dashboard-vX.zip
cd pentest-dashboard-vX
sudo bash update.sh
```

Reports are always preserved during updates.

---

## Manual Install (no installer)

If you prefer to run without systemd:

```bash
sudo apt update
sudo apt install -y nmap nikto whois dnsutils curl whatweb sslscan python3 python3-pip
pip3 install flask --break-system-packages

# Run directly
sudo python3 app.py

# Access at http://<pi-ip>:5000
```

---

## Firewall

The installer opens port 5000 automatically if UFW is active.  
To manage manually:

```bash
sudo ufw allow 5000/tcp    # allow
sudo ufw deny  5000/tcp    # block
sudo ufw status            # check
```

---

## Uninstall

```bash
sudo bash uninstall.sh
```

You'll be asked whether to keep your saved reports before removal.

---

## System Requirements

- Raspberry Pi 4 (2 GB RAM minimum; 4 GB recommended for heavy scans)
- Kali Linux (any recent image, 2023+)
- Python 3.9+
- Internet or LAN access for scanning

---

## ⚠ Legal Disclaimer

This tool is for **authorized penetration testing only**.  
Only scan systems you **own** or have **explicit written permission** to test.  
Unauthorized port scanning or vulnerability scanning may be illegal in your jurisdiction.  
The authors accept no liability for misuse.
