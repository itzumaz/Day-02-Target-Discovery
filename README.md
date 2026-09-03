# Day 2: Target Discovery & Service Version Detection

## 📌 Project Overview
This repository documents the execution and methodology for **Day 2 of my Vulnerability Assessment and Penetration Testing (VAPT) Internship with TriosCyber** (in partnership with Ernith). 

The focus of this phase was setting up an authorized host lab environment, verifying host-to-host network connectivity, mapping the active network attack surface, and performing precise service version detection across exposed ports on the target machine.

---

## 🛠️ Lab Environment & Topology
* **Attacker System:** Kali Linux (`192.168.145.128`)
* **Target System:** Metasploitable 2 (`192.168.145.132`)
* **Network Interface:** Virtual Network (VMware)
* **Tools Utilized:** Nmap, ICMP Ping

---

## 🚀 Execution & Methodology

### Step 1: Target Discovery & Network Connectivity Verification
To confirm that the virtual network architecture was communicating properly before scanning, a standard ICMP ping utility test was executed between the local attacker host and the target system.
```bash
ping -c 4 192.168.145.132
```
* **Result:** Packet transmission was successful with 0% packet loss, establishing a verified link.

---

### Step 2: Basic Nmap Host & Port Scan
A standard TCP Nmap scan was initiated to trace and list all active open ports running on the target machine.
```bash
nmap 192.168.145.132
```

**Discovered Open Ports & Services:**
The initial scan exposed a highly insecure footprint consisting of numerous open legacy services:
* `21/tcp` - FTP
* `22/tcp` - SSH
* `23/tcp` - Telnet
* `25/tcp` - SMTP
* `53/tcp` - Domain (DNS)
* `80/tcp` - HTTP
* `111/tcp` - rpcbind
* `139/tcp` - netbios-ssn
* `445/tcp` - microsoft-ds (SMB)
* `512/tcp`, `513/tcp`, `514/tcp` - exec / login / shell
* `1099/tcp` - rmiregistry
* `1524/tcp` - ingreslock
* `2049/tcp` - nfs
* `2121/tcp` - ccproxy-ftp
* `3306/tcp` - mysql
* `5432/tcp` - postgresql
* `5900/tcp` - vnc
* `6000/tcp` - X11
* `6667/tcp` - irc
* `8009/tcp` - ajp13
* `8180/tcp` - HTTP-alt

---

### Step 3: Service Version Detection
To identify the exact software, daemons, and operating system build information running behind the open ports, an advanced Nmap service version detection scan (`-sV`) was completed.
```bash
nmap -sV 192.168.145.132
```

**Key Service Details Identified:**
The scan identified specific software version signatures known to contain severe, unpatched security vulnerabilities:

| Port | Service | Detected Software Version |
| :--- | :--- | :--- |
| **21** | FTP | `vsftpd 2.3.4` |
| **22** | SSH | `OpenSSH 4.7p1 Debian 8ubuntu1` |
| **23** | Telnet | `Linux telnetd` |
| **25** | SMTP | `Postfix smtpd` |
| **53** | DNS | `ISC BIND 9.4.2` |
| **80** | HTTP | `Apache httpd 2.2.8 ((Ubuntu) DAV/2)` |
| **445** | SMB | `Samba smbd 3.X - 4.X` |

---

## 🛡️ Remediation & Defensive Recommendations
The discovery of cleartext protocols and heavily outdated service versions poses a critical threat to an enterprise network. To resolve these findings from your end, apply these standard defensive configurations:

* **Replace Insecure Cleartext Protocols:**
  Permanently disable Telnet (Port 23). Telnet passes all login credentials and session traffic across the network in plaintext. Enforce the use of encrypted protocols like SSH (Port 22) for all remote command line management.
* **Mitigate Banner Grabbing Exploits:**
  Nmap successfully extracted detailed version data (e.g., `Apache 2.2.8`, `vsftpd 2.3.4`) because the server banners are explicitly exposing software specifics. Configure application files (such as setting `ServerTokens ProductOnly` and `ServerSignature Off` inside Apache configuration files) to obscure version strings from unauthorized scanners.
* **Decommission Deprecated Software Builds:**
  The target runs legacy infrastructure versions that are no longer supported by vendors. Modernize the system architecture by tracking down software update paths or building updated virtual machine configurations to remove foundational code exploits.

---

## 🧠 Key Takeaways
* **Reconnaissance Foundation:** Successful service enumeration forms the blueprint for all subsequent security phases, ensuring an analyst maps exactly what needs to be protected or patched.
* **Information Leakage Risks:** Default machine setups often announce precise system information to anyone querying the network ports, lowering the barrier to entry for potential attacks.

---

## 📂 Repository Structure
```plaintext
.
├── README.md
└── screenshots/
    ├── basic_nmap_port_scan.png
    └── nmap_service_version_detection.png
```

---

## 👤 Author
**Azeez Umar Opeyemi**
* 💼 **Role:** VAPT Intern at TriosCyber
* 📧 **Email:** umaropeyemiazeez@gmail.com
* 🐙 **GitHub:** [itzumaz](https://github.com/itzumaz)
*   **LinkedIn:** [Azeez Umar Opeyemi](https://www.linkedin.com/in/azeez-umar-opeyemi-201a433a4/)