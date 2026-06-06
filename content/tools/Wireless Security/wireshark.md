---
title: 'WIRESHARK - Complete Network Traffic Analysis Guide'
date: 2026-06-05
draft: false
---


> **Network Protocol Analyzer: Capture and inspect network packets in real-time**

---

## Table of Contents
1. [What is Wireshark?](#what-is-wireshark)
2. [Installation](#installation)
3. [Basic Setup](#basic-setup)
4. [Capturing Packets](#capturing-packets)
5. [Display Filters](#display-filters)
6. [Protocol Analysis](#protocol-analysis)
7. [Real-World Examples](#real-world-examples)
8. [Interview Questions](#interview-questions)

---

## What is Wireshark?

Wireshark is the most popular **packet sniffer** and **network analyzer**. It allows you to:
- Capture live network traffic
- See exactly what's being transmitted on network
- Analyze protocols (HTTP, HTTPS, DNS, FTP, etc.)
- Extract files from network traffic
- Find credentials, passwords, API keys
- Troubleshoot network issues
- Identify security threats

---

## Installation

```bash
# Ubuntu/Debian
sudo apt-get install wireshark

# Kali Linux
sudo apt-get install wireshark

# macOS
brew install wireshark

# Verify
wireshark --version

# Post-installation (Linux)
# Add your user to wireshark group to avoid sudo
sudo usermod -aG wireshark $USER
sudo chmod +x /usr/bin/dumpcap
```

---

## Basic Setup

### Running Wireshark

```bash
# GUI mode (easiest)
wireshark &

# Capture from terminal (tshark)
tshark -i eth0

# Capture and save to file
tshark -i eth0 -w capture.pcap
```

### Interface Selection

- Click the **blue shark icon** to start capture
- Select network interface (eth0, wlan0, etc.)
- Click **Start** to begin capturing packets

---

## Capturing Packets

### Basic Capture

```
1. Select interface (Wi-Fi or Ethernet)
2. Click shark icon to start
3. Generate traffic (browse web, etc.)
4. Click stop to end capture
```

### Capture Filters (Before capture starts)

```bash
# Only capture on port 80 (HTTP)
port 80

# Only capture HTTP traffic
tcp port 80

# Only capture to specific IP
host 192.168.1.100

# Only capture from specific subnet
net 192.168.1.0/24

# Exclude specific traffic
not port 22

# Multiple conditions (AND)
host 192.168.1.100 and port 80

# Multiple conditions (OR)
port 80 or port 443
```

---

## Display Filters (Most Important!)

### Basic Filters

```
# Filter by protocol
http
https
dns
ftp
smtp
ssh
telnet
snmp

# Filter by port
tcp.port == 80
udp.port == 53

# Filter by IP address
ip.src == 192.168.1.100
ip.dst == 8.8.8.8
ip.addr == 192.168.1.100

# Filter by MAC address
eth.src == 00:11:22:33:44:55
eth.dst == aa:bb:cc:dd:ee:ff
```

### Intermediate Filters

```
# HTTP requests only
http.request

# HTTP responses only
http.response

# DNS queries
dns.qry.name

# DNS responses
dns.resp.name

# SSL/TLS handshake
ssl.handshake

# TCP connections
tcp.flags.syn == 1 (SYN packets)
tcp.flags.ack == 1 (ACK packets)
tcp.flags.fin == 1 (FIN packets)

# Filter by protocol AND port
tcp.port == 443 and ssl

# Exclude certain traffic
!dns and !arp
```

### Advanced Filters

```
# Find packets with specific string in payload
tcp contains "password"
http contains "admin"

# Find non-standard HTTP ports
tcp.port == 8080 and http

# Find packets larger than 1MB
frame.len > 1000000

# Find packets in time range
frame.time >= "2024-01-01 00:00:00"

# Find HTTPS traffic
ssl or tls

# Find unencrypted credentials
http.authorization or ftp.user

# Combination filters
(ip.src == 192.168.1.100 or ip.src == 192.168.1.101) and tcp.port == 80
```

---

## Protocol Analysis

### HTTP Analysis

```
1. Filter: http.request
2. Look for GET/POST requests
3. Check Request URI (URL being accessed)
4. Right-click → Follow → HTTP Stream
5. See plaintext request and response

# Important fields:
- http.host: Target domain
- http.user_agent: Browser/software
- http.request.method: GET/POST/PUT/DELETE
- http.request.uri: Path being accessed
- http.response.code: Status (200/404/500 etc.)
```

### HTTPS/TLS Analysis

```
1. Filter: ssl or tls
2. Look for handshake packets
3. Check certificate information
4. Analyze cipher suites

# Can see:
- Server certificate
- Cipher suite used
- TLS version (1.0, 1.2, 1.3)
- But NOT the encrypted data itself
```

### DNS Analysis

```
1. Filter: dns
2. Look for dns.qry.name (queries)
3. Look for dns.resp.name (responses)
4. See what domains target is accessing

# Example:
# Client queries: google.com
# Server responds: 142.250.185.46
# Now you know Google's IP from this time
```

### FTP Analysis

```
1. Filter: ftp or ftp-data
2. Look for USER command
3. Look for PASS command (PASSWORD IN PLAINTEXT!)
4. Follow FTP stream to see credentials
5. See file transfers

# FTP is unencrypted - passwords visible!
```

### SMTP (Email) Analysis

```
1. Filter: smtp
2. See MAIL FROM: sender
3. See RCPT TO: recipient
4. See email headers and content

# Can extract:
- Sender email
- Recipient email
- Subject
- Full email body (if sent plaintext)
```

---

## Real-World Scenarios

### Scenario 1: Find Web Credentials

```
1. Start Wireshark
2. Use filter: http.user_agent
3. User accesses web application
4. Look for POST requests
5. Right-click POST → Follow → HTTP Stream
6. Can see username/password if sent as plaintext!

# Why this works:
HTTP has no encryption. Everything visible.
```

### Scenario 2: Identify DNS Queries

```
1. Filter: dns
2. Set capture on home network
3. See what websites everyone accessed
4. Analyze dns.qry.name field

# Example output:
# Host queries: facebook.com, youtube.com, reddit.com
# Shows browsing habits even without seeing actual sites
```

### Scenario 3: Extract Files from Traffic

```
1. Start capture on network
2. Someone downloads file
3. Filter by file protocol (http, ftp, smb)
4. Right-click stream → Export Objects → HTTP
5. Save the downloaded file from capture!

# Works for:
# Images, PDFs, executables, documents
```

### Scenario 4: Detect Network Scanning

```
1. Wathcatcher looking for nmap scan
2. Filter: tcp.flags.syn
3. See multiple SYN packets to different ports
4. All from same source IP
5. Indicates network scan in progress
```

### Scenario 5: Certificate Inspection

```
1. Filter: ssl.handshake
2. User visits HTTPS website
3. Right-click packet → Wireshark → Follow → SSL
4. Expand SSL packet
5. See server certificate, issuer, expiry, public key
```

---

## Advanced Features

### Coloring Rules

```
Right-click packet → Set Color → Choose color
Or: View → Coloring Rules

Color coding helps:
- Red: Errors, RST packets
- Green: Good traffic
- Orange: Warnings
- Blue: TCP
- Pink: UDP
```

### Statistics

```
Menu → Statistics → Conversations
- See all IP pairs talking to each other
- See data volumes
- See packet counts

Menu → Statistics → Protocol Hierarchy
- See all protocols used
- See percentage breakdown
```

### Following Streams

```
Right-click packet → Follow → TCP Stream
- See complete conversation
- Both directions
- Plaintext (if no encryption)

Colors:
- Blue: Client → Server
- Red: Server → Client
```

---

## Interview Questions & Answers

### Q1: How would you find a user's password on the network?

**A:**
```bash
# Use filter for unencrypted protocols
http.user_agent or ftp or telnet

# HTTP login (plaintext)
Filter: http.request.method == POST
Right-click → Follow → HTTP Stream
See: username=admin&password=secret123

# FTP login (plaintext)
Filter: ftp
Look for: USER and PASS commands
Both visible in plaintext

# Why this works: HTTP/FTP unencrypted
# HTTPS/SSH are encrypted - can't see credentials
```

---

### Q2: What's the difference between Capture and Display filters?

**A:**
- **Capture Filter**: Applied BEFORE capture starts
  - Reduces file size (only captures matching packets)
  - Applied at packet capture level
  - Faster (filters before storage)
  
- **Display Filter**: Applied AFTER capture starts
  - Shows/hides packets from existing capture
  - Can change anytime without recapturing
  - Doesn't reduce file size

---

### Q3: How would you extract a file from network traffic?

**A:**
```
1. Start Wireshark capture
2. Someone downloads file
3. Stop capture after download
4. Menu: File → Export Objects → HTTP (or protocol)
5. Select file from list
6. Click Save
7. File extracted and ready to use
```

---

### Q4: Explain DNS spoofing and how Wireshark detects it

**A:**
```
Normal DNS:
1. Client: queries google.com
2. Server responds: 142.250.185.46

DNS spoofing:
1. Client: queries google.com
2. Attacker intercepts: sends 192.168.1.50
3. Client connects to attacker's IP

Detection in Wireshark:
1. Filter: dns.resp.name
2. Know legitimate Google IP (142.250.185.46)
3. If response shows different IP → SPOOFING
4. Cross-reference with known IPs
```

---

### Q5: How would you find unencrypted credentials on network?

**A:**
```bash
# Filter for unencrypted protocols
tcp contains "password" or tcp contains "username"

# Or target specific protocols
ftp or telnet or http.authorization

# Or look for login attempts
filter: http.request.method == POST and http.request.uri contains "login"

# Then follow stream to see actual credentials
```

---

### Q6: What information can you extract from HTTPS?

**A:**
```
Can see:
✅ Certificate information (domain, issuer, expiry)
✅ Server IP
✅ Cipher suites used
✅ TLS version
✅ Public key
✅ Certificate chain

Cannot see:
❌ Actual data being transmitted (encrypted)
❌ Passwords
❌ Usernames
❌ URLs
❌ Website content
```

Good luck with interviews! 🎯
