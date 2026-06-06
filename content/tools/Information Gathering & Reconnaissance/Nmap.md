---
title: 'NMAP - Complete Network Mapping Guide'
date: 2026-06-05
draft: false
---


> **Network Mapper: Discover hosts, ports, services, and vulnerabilities on your network**

---

## Table of Contents
1. [What is Nmap?](#what-is-nmap)
2. [Installation](#installation)
3. [Basic Scans](#basic-scans)
4. [Scan Types](#scan-types)
5. [Advanced Options](#advanced-options)
6. [Real-World Examples](#real-world-examples)
7. [Interview Questions](#interview-questions)

---

## What is Nmap?

Nmap (Network Mapper) is a free, open-source tool for **network discovery and security auditing**. It answers questions like:
- What hosts are running on the network?
- What ports are open?
- What services are running?
- What OS is the target running?
- Are there vulnerabilities?

---

## Installation

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install nmap

# Kali Linux
sudo apt-get install nmap

# macOS
brew install nmap

# Verify installation
nmap --version
```

---

## Basic Scans

### Basic Syntax
```
nmap [Scan Type] [Options] [Target]
```

### Simple Ping Scan (Host Discovery)
```bash
# Ping a single host
nmap -sn 192.168.1.1

# Ping a range
nmap -sn 192.168.1.0/24

# What it does: Sends ICMP packets to see if hosts are alive
# Output: Lists all responsive hosts (no port scanning)
```

### Basic Port Scan
```bash
# Scan default 1000 ports
nmap 192.168.1.100

# Scan all 65535 ports (takes time)
nmap -p- 192.168.1.100

# Scan specific ports
nmap -p 80,443,22 192.168.1.100

# Scan port range
nmap -p 1-1000 192.168.1.100

# Scan top ports
nmap --top-ports 100 192.168.1.100
```

### Service Detection
```bash
# Detect service versions
nmap -sV 192.168.1.100

# Example output:
# 80/tcp   open  http    Apache httpd 2.4.41
# 443/tcp  open  https   Apache httpd 2.4.41
# 22/tcp   open  ssh     OpenSSH 7.6p1
```

### OS Detection
```bash
# Aggressive OS detection
nmap -O 192.168.1.100

# Output:
# Running: Linux 4.x
# Aggressive OS guesses: Linux 4.15-4.19 (95%)

# Note: Requires root/sudo privileges
```

---

## Scan Types (Very Important for Interviews)

### TCP Scans

#### TCP Connect Scan (-sT)
```bash
nmap -sT 192.168.1.100

# What: Completes full TCP 3-way handshake
# Speed: Slow (establishes full connection)
# Logging: Visible in server logs
# Permission: Works without root
# Use: Stealth not required
```

#### TCP SYN Scan (-sS) - STEALTH SCAN
```bash
nmap -sS 192.168.1.100

# What: Sends SYN packet, doesn't complete handshake
# Speed: Fast
# Logging: Less visible (not full connection)
# Permission: Requires root
# Use: Default scan, best balance

# Packet flow:
# 1. Send SYN to port
# 2. If SYN-ACK received → port OPEN
# 3. If RST received → port CLOSED
# 4. If no response → port FILTERED
```

#### TCP ACK Scan (-sA)
```bash
nmap -sA 192.168.1.100

# What: Doesn't determine if port is open
# Purpose: Map firewall rules (which ports are filtered)
# Output: filtered vs unfiltered
# Use: Firewall reconnaissance
```

#### TCP NULL, FIN, Xmas Scans (-sN, -sF, -sX)
```bash
# NULL Scan - sends packets with no flags
nmap -sN 192.168.1.100

# FIN Scan - sends packets with FIN flag
nmap -sF 192.168.1.100

# Xmas Scan - sends packets with FIN, PSH, URG flags
nmap -sX 192.168.1.100

# All three work on RFC: closed ports send RST
# Open/filtered ports don't respond
# Use: Evade basic firewalls
# Caveat: Only works on systems following RFC strictly
```

### UDP Scans

```bash
# UDP scan (slow, but finds DNS, SNMP, etc.)
nmap -sU 192.168.1.100

# What: Sends UDP packets, waits for ICMP unreachable
# Speed: Very slow (needs timeout)
# Use: Services like DNS (53), SNMP (161), DHCP (67)

# Combine TCP and UDP
nmap -sS -sU 192.168.1.100
```

### PING Scan Only

```bash
# -sn: Ping scan only (no port scanning)
nmap -sn 192.168.1.0/24

# Output: Just lists alive hosts
# Use: Network discovery without port scan
```

---

## Timing and Performance

### Timing Templates (from paranoid to insane)

```bash
# -T0 (Paranoid): Very slow, stealthy
nmap -T0 192.168.1.100

# -T1 (Sneaky): Slow, stealthy
nmap -T1 192.168.1.100

# -T2 (Polite): Slower, doesn't impact target
nmap -T2 192.168.1.100

# -T3 (Normal): Default, balanced
nmap -T3 192.168.1.100

# -T4 (Aggressive): Fast, modern networks
nmap -T4 192.168.1.100

# -T5 (Insane): Very fast, may miss services
nmap -T5 192.168.1.100

# Real-world tip:
# Use T4 for internal networks (fast)
# Use T1 for external targets (stealth)
```

---

## Advanced Options

### Verbosity & Debugging

```bash
# Increase verbosity (show more details)
nmap -v 192.168.1.100

# Very verbose
nmap -vv 192.168.1.100

# Debug mode
nmap -d 192.168.1.100

# Very debug
nmap -dd 192.168.1.100
```

### Output Formats

```bash
# Normal output (default)
nmap 192.168.1.100

# Save to file
nmap 192.168.1.100 -oN output.txt

# XML format (for parsing)
nmap 192.168.1.100 -oX output.xml

# Greppable format (easy to grep)
nmap 192.168.1.100 -oG output.grep

# All formats at once
nmap 192.168.1.100 -oA output
# Creates: output.nmap, output.xml, output.gnmap

# Append to file (don't overwrite)
nmap 192.168.1.100 -oN output.txt --append-output
```

### Script Scanning (Vulnerability Detection)

```bash
# Default scripts (safe)
nmap -sC 192.168.1.100

# Specific script
nmap --script http-title 192.168.1.100

# Multiple scripts
nmap --script http-title,http-headers 192.168.1.100

# All scripts in category
nmap --script vuln 192.168.1.100

# Scripts with output
nmap --script http-title --script-args script-args.cmd

# List available scripts
ls /usr/share/nmap/scripts/

# Update scripts
nmap --script-updatedb
```

### Common Script Categories

```bash
# Vulnerability detection
nmap --script vuln 192.168.1.100

# HTTP enumeration
nmap --script http-* 192.168.1.100

# SSL/TLS detection
nmap --script ssl-* 192.168.1.100

# SMB enumeration (Windows)
nmap --script smb-* 192.168.1.100

# DNS enumeration
nmap --script dns-* 192.168.1.100

# Default credentials
nmap --script default 192.168.1.100
```

---

## Real-World Examples

### Example 1: Complete Network Reconnaissance
```bash
# Comprehensive scan
nmap -sS -sV -sC -O -p- --script vuln 192.168.1.100 -oA recon

# What this does:
# -sS: SYN scan (fast, stealthy)
# -sV: Detect service versions
# -sC: Run default scripts
# -O: Detect OS
# -p-: All ports
# --script vuln: Vulnerability detection
# -oA recon: Save all formats
```

### Example 2: Quick Internal Network Scan
```bash
# Fast scan of network
nmap -T4 -F 192.168.1.0/24

# What this does:
# -T4: Aggressive timing
# -F: Fast mode (only top 100 ports)
# /24: Entire subnet
```

### Example 3: Stealth Scan (Slow)
```bash
# Very stealthy (slow)
nmap -sS -T1 -p1-10000 --script vuln 192.168.1.100

# What this does:
# -sS: SYN scan
# -T1: Sneaky timing
# -p1-10000: First 10k ports
# --script vuln: Vulnerability checks
```

### Example 4: Firewall Evasion
```bash
# Fragment packets
nmap -f 192.168.1.100

# Decoy scan (hide real IP among fake ones)
nmap -D 192.168.1.50,192.168.1.51,ME 192.168.1.100

# Idle scan (use zombie host)
nmap -sI 192.168.1.50 192.168.1.100

# Randomize ports
nmap -p- --randomize-hosts 192.168.1.100
```

---

## Output Interpretation

```
PORT      STATE    SERVICE      VERSION
────────────────────────────────────────
22/tcp    open     ssh          OpenSSH 7.6p1
80/tcp    open     http         Apache httpd 2.4.41
443/tcp   open     https        nginx
3306/tcp  closed   mysql
8080/tcp  filtered http-proxy

# Meanings:
# open: Service is listening and accepting connections
# closed: Port is accessible but no service (unlikely)
# filtered: Firewall blocking - can't determine state
# open|filtered: Likely open but unclear
```

---

## Interview Questions & Answers

### Q1: What's the difference between -sS and -sT scans?

**A:**
- **-sT (Connect Scan)**: 
  - Completes full TCP 3-way handshake
  - Doesn't require root
  - Visible in server logs
  - Slower

- **-sS (SYN Scan)**:
  - Sends SYN, receives SYN-ACK, sends RST (doesn't complete)
  - Requires root
  - Stealthier, less logging
  - Faster (default for root)

---

### Q2: When would you use -T1 vs -T4?

**A:**
- **-T1 (Sneaky)**: External targets, want to avoid detection
- **-T4 (Aggressive)**: Internal networks, modern infrastructure, speed is priority
- Trade-off: Speed vs Stealth

---

### Q3: How would you enumerate all services on a network?

**A:**
```bash
# Step 1: Find all hosts
nmap -sn 192.168.1.0/24 -oG hosts.txt

# Step 2: Scan all hosts with service detection
nmap -sV -p- -iL hosts.txt -oA detailed_scan

# Step 3: Look for vulnerabilities
nmap --script vuln -iL hosts.txt
```

---

### Q4: Explain the TCP handshake and SYN scan

**A:**
```
Normal TCP handshake:
1. Client → Server: SYN (want to connect)
2. Server → Client: SYN-ACK (acknowledging)
3. Client → Server: ACK (connected)

SYN scan exploit:
1. Send SYN to port
2. Receive SYN-ACK → port OPEN
3. Send RST to close (don't complete handshake)
4. Never send final ACK (stealth)
```

---

### Q5: How would you find a web server running on non-standard port?

**A:**
```bash
# Scan all ports with service detection
nmap -sV -p- 192.168.1.100

# Or target specific ranges
nmap -sV -p 1-10000 192.168.1.100

# Look for http/https in service column
```

Good luck! 🎯
