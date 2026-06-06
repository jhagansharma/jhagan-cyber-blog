---
title: 'IntelRecon-OSINT-Automation-Tool'
date: 2026-06-05
draft: false
tags: ["forensics", "metadata", "python"]
---
> **A modular, menu-driven OSINT framework that automates intelligence gathering from publicly available sources while maintaining strict legal and ethical standards**

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [The Problem It Solves](#the-problem-it-solves)
3. [Investigation Modules](#investigation-modules)
4. [Architecture](#architecture)
5. [Technology Stack](#technology-stack)
6. [API Integration](#api-integration)
7. [Safety & Legal Compliance](#safety--legal-compliance)
8. [Output Format](#output-format)
9. [Interview Q&A](#interview-qa)

---

## Project Overview

**OSINT** = **Open Source Intelligence** — gathering information from publicly available sources.

The **OSINT Investigation Framework** is a command-line tool that automates intelligence gathering about:
- Domains and websites
- IP addresses
- Email addresses
- Phone numbers
- Social media usernames
- Vehicle registration numbers (India-specific)
- File metadata

Instead of manually running 20+ different tools and compiling results, investigators type one command and get a comprehensive report.

### Quick Stats
- **Language**: Python 3
- **Modules**: 7 investigation types
- **Tools Integrated**: 20+ OSINT tools
- **Output Formats**: TXT + JSON reports
- **Optional APIs**: 9 (all optional - framework works without them)
- **Target Users**: Law enforcement, cybercrime units, authorized investigators
- **Legal**: Passive-only by default, active methods require user confirmation

---

## The Problem It Solves

### Scenario: Traditional OSINT Investigation

```
Investigator needs to investigate: suspicious-domain.com

❌ BEFORE (Manual Process):
├─ Run whois command
│  └─ Write down results manually
├─ Run dig command
│  └─ Parse DNS output manually
├─ Run nmap for ports
│  └─ Interpret port scan results
├─ Check VirusTotal (if you have API)
│  └─ Copy-paste results
├─ Search Shodan (if you have API)
│  └─ Take screenshots
├─ Check SSL certificates
│  └─ Document findings
├─ Research technologies (whatweb)
│  └─ Manual analysis
└─ Compile into report
   └─ 2-3 hours of work ⏱️

✅ WITH OSINT Framework:
python3 osint.py
  → Select: Domain OSINT
  → Enter: suspicious-domain.com
  → Wait 5-10 minutes
  → Professional report generated
└─ 10 minutes ⚡
```

### Major Challenges Addressed

1. **Too Many Tools**
   - Investigators need to learn 20+ different tools
   - Each has different output format
   - Each requires different installation
   - Hard to remember command syntax

2. **Manual Data Compilation**
   - Copy-pasting results from tools
   - Inconsistent formatting
   - Errors when manually entering data
   - Time-consuming verification

3. **Incomplete Information**
   - Single tool gives partial picture
   - Need multiple tools for complete intel
   - Correlation requires manual work
   - Easy to miss connections

4. **API Fragmentation**
   - Different APIs for different tools
   - Need different accounts/keys
   - Some require payment
   - No unified interface

**Solution**: Unified interface that:
- Runs all tools automatically
- Compiles results consistently
- Correlates information across sources
- Includes optional API enrichment
- Generates professional reports

---

## Investigation Modules

### Module 1: Domain OSINT

**Purpose**: Investigate websites and domain ownership

**Use Cases**:
- Is this domain legitimate or malicious?
- Who owns this domain?
- What servers does it use?
- What technologies power it?
- Is it being used for scams?

**Tools Run**:
```
whois     → Domain registration details
dig       → DNS records (A, MX, NS, TXT)
host      → Hostname resolution
dnsenum   → DNS enumeration
dnsrecon  → Advanced DNS reconnaissance
amass     → Subdomain discovery
crt.sh    → SSL certificate history
whatweb   → Technology identification
sslscan   → SSL/TLS analysis
nmap*     → Port scanning (*requires confirmation)
```

**Example Output**:
```
DOMAIN OSINT REPORT: suspicious-site.com
═════════════════════════════════════════════════════════

REGISTRAR INFORMATION
Registrar: GoDaddy.com
Domain:    suspicious-site.com
Created:   2023-01-15
Expires:   2025-01-15
Updated:   2024-06-01
Registrant: REDACTED (Privacy Protected)

DNS RECORDS
A Record:        185.24.133.45
MX Record:       mail.suspicious-site.com
NS Records:      ns1.godaddy.com, ns2.godaddy.com
TXT Records:     v=spf1 include:_spf.google.com
                 google-site-verification=abc123xyz

SUBDOMAINS FOUND
mail.suspicious-site.com        (185.24.133.45)
www.suspicious-site.com         (185.24.133.45)
admin.suspicious-site.com       (185.24.133.45)
api.suspicious-site.com         (SEPARATE IP: 192.168.1.1)

SSL CERTIFICATE
Issued To:      suspicious-site.com
Issued By:      Let's Encrypt
Valid From:     2024-01-01
Valid Until:    2025-01-01
Issuer CN:      R3

TECHNOLOGIES IDENTIFIED
Web Server:     Nginx 1.21.6
CMS:            WordPress 5.8
Scripting:      PHP 7.4
JavaScript:     jQuery 3.6.0

OPEN PORTS (Port Scan)
80   - HTTP       (open)
443  - HTTPS      (open)
22   - SSH        (closed)
3306 - MySQL      (closed)

API ENRICHMENT (VirusTotal)
Status:          MALICIOUS
Threat:          Phishing Domain
Detection Rate:  23/83 vendors
Last Analysis:   2024-06-05 14:23:45

API ENRICHMENT (Shodan)
IP:              185.24.133.45
Organization:    DigitalOcean
City:            Amsterdam
Ports:           80, 443, 8080
Services:        HTTP, HTTPS, Apache Proxy
```

---

### Module 2: IP Address OSINT

**Purpose**: Investigate IP addresses and their services

**Use Cases**:
- Where is this server located?
- What services are running?
- Is this IP malicious?
- Who owns this IP?
- What devices are connected?

**Tools Run**:
```
whois        → IP ownership and ASN
geoiplookup  → Geographic location
ipcalc       → Subnet information
dig          → Reverse DNS lookup
host         → Hostname resolution
traceroute   → Network path
nmap*        → Open ports and services
```

**Example Output**:
```
IP OSINT REPORT: 185.24.133.45
═════════════════════════════════════════════════════════

IP OWNERSHIP
IP Address:       185.24.133.45
Organization:     DigitalOcean Inc
ASN:              AS14061
Country:          Netherlands
City:             Amsterdam
ISP:              DigitalOcean

GEOLOCATION
Latitude:         52.3676
Longitude:        4.9041
City:             Amsterdam
Country:          Netherlands
Region:           North Holland
Timezone:         Europe/Amsterdam
ISP:              DigitalOcean

REVERSE DNS
IP:               185.24.133.45
Hostname:         mail.vhost.com
PTR Record:       185-24-133-45.cloud.digitalocean.com

SUBNET INFORMATION
Network:          185.24.0.0/16
Broadcast:        185.24.255.255
First Host:       185.24.0.1
Last Host:        185.24.255.254
Total Hosts:      65,535

OPEN PORTS & SERVICES
Port 80:          HTTP (Apache/2.4.41)
Port 443:         HTTPS (Certificate valid until 2025-01-01)
Port 22:          SSH (OpenSSH 7.6)
Port 25:          SMTP (Postfix 3.4.8)
Port 3306:        MySQL (5.7.29)

NETWORK PATH (Traceroute)
Hop 1:  192.168.1.1 (Your Router)
Hop 2:  10.0.0.1 (ISP Gateway)
Hop 3:  AS1234 Core Router
Hop 4:  AS1234 Edge Router
Hop 5:  AS14061 DigitalOcean Entry
Hop 6:  185.24.133.45 (Target)

API ENRICHMENT (Shodan)
Services:        HTTP, HTTPS, SSH, SMTP, MySQL
Banners:         Apache/2.4.41, OpenSSH 7.6
Vulnerabilities: CVE-2021-41773 (Apache)
Last Scan:       2024-06-03

API ENRICHMENT (AbuseIPDB)
Abuse Reports:   12
Total Reports:   3,456
Confidence:      78%
Last Report:     2024-06-02
Reports From:    Unauthorized login attempts, Port scanning, SSH brute force
```

---

### Module 3: Email OSINT

**Purpose**: Gather intelligence about email addresses

**Use Cases**:
- Is this email address real?
- Has this email been compromised?
- What accounts use this email?
- What domains is it registered on?

**Tools Run**:
```
dig            → MX/SPF/DMARC records
whois          → Domain owner info
holehe         → Account existence check
sherlock       → Username across platforms
HaveIBeenPwned → Breach database
Hunter.io      → Email verification
```

**Example Output**:
```
EMAIL OSINT REPORT: suspect@example.com
═════════════════════════════════════════════════════════

EMAIL VALIDATION
Format Valid:     ✓ Yes
Domain Exists:    ✓ Yes
MX Records:       aspmx.l.google.com, alt1.aspmx.l.google.com
Email Provider:   Google Workspace

BREACH HISTORY
In Breaches:      ✓ YES (3 breaches)

Breach 1: LinkedIn (2012)
├─ Date:       2012-05-05
├─ Records:    165 million
├─ Exposed:    Email, Password Hash
└─ Status:     Confirmed

Breach 2: Adobe (2013)
├─ Date:       2013-10-03
├─ Records:    153 million
├─ Exposed:    Email, Password Hash
└─ Status:     Confirmed

Breach 3: Yahoo (2014)
├─ Date:       2014-12-14
├─ Records:    500 million
├─ Exposed:    Email, Password, Security Questions
└─ Status:     Confirmed

ASSOCIATED ACCOUNTS
Found on Platforms:
├─ GitHub       user_github_123 (15 repos)
├─ Twitter      @suspect_user (2,300 followers)
├─ LinkedIn     suspect-user (Sr. Software Engineer)
├─ Facebook     suspect.user.123 (Private)
├─ Instagram    suspect_user_ (482 followers)
├─ Reddit       u/suspect_user (5,000 karma)
├─ Medium       suspect-user (Technology writer)
└─ StackOverflow suspect_user (5,000 reputation)

DOMAIN INFORMATION
Domain:         example.com
Registrant:     John Doe
Registration:   2010-03-15
Expiration:     2025-03-15
Registrar:      GoDaddy.com
Status:         Active

RECOMMENDATIONS
[⚠] Email appears in 3 known breaches
[⚠] Associated accounts reveal personal information
[✓] Password should be changed (LinkedIn breach old, others recent)
[✓] Enable 2-factor authentication
```

---

### Module 4: Phone Number OSINT

**Purpose**: Investigate phone numbers and carriers

**Use Cases**:
- What country is this number from?
- What carrier operates it?
- Is it a valid number?
- What's the location?

**Tools Run**:
```
Country Detection  → Identify country from code
Carrier Detection  → Identify telecom operator
PhoneInfoga        → Phone intelligence
NumVerify          → Phone validation
Twilio             → Carrier details
```

**Example Output**:
```
PHONE OSINT REPORT: +91-9876543210
═════════════════════════════════════════════════════════

PHONE VALIDATION
Number:           +91-9876543210
Format Valid:     ✓ Yes
Number Valid:     ✓ Yes
In Use:           ✓ Likely

LOCATION INFORMATION
Country:          India
Country Code:     +91
State/Region:     Delhi
City:             New Delhi

CARRIER INFORMATION
Primary Carrier:  Vodafone Idea Limited
Carrier Type:     Mobile (Cellular)
Network:          GSM/3G/4G LTE
Coverage:         Pan-India

NUMBER DETAILS
Type:             Mobile
Activation Date:  2018-06-15
Status:           Active
Last Activity:    2024-06-05

INTELLIGENCE SUMMARY
├─ This is an active Indian mobile number
├─ Operated by Vodafone Idea Limited
├─ Registered to Delhi region
└─ Recent activity confirmed

API ENRICHMENT (Twilio)
Carrier:          VODAFONE IDEA LIMITED
Type:             mobile
Portable:         False
Network Type:     cellular
```

---

### Module 5: Vehicle OSINT (India-Specific)

**Purpose**: Lookup Indian vehicle registration numbers

**Use Cases**:
- What does this registration plate mean?
- Where was vehicle registered?
- What's the registration year?
- How old is this vehicle?

**Tools Run**:
```
RTO Database    → Regional Transport Office
Format Analysis → Decode registration plate
Parivahan Ref   → Ministry of Road Transport
```

**Example Output**:
```
VEHICLE OSINT REPORT: DL-01-AB-1234
═════════════════════════════════════════════════════════

REGISTRATION BREAKDOWN
State:            DL (Delhi)
District Code:    01 (Delhi District)
Letter Series:    AB (Sequential allocation)
Number Sequence:  1234 (Sequential)

VEHICLE INFORMATION
Registration Year: 2001
Vehicle Age:       23 years
Category:          Private Car / Taxi
Status:            Active

RTO DETAILS
RTO Office:       New Delhi
RTO Code:         DL-01
Region:           Delhi
Jurisdiction:     National Capital Region

REGISTRATION HISTORY
First Registered:  2001-01-15
Last Renewed:      2024-06-01
Renewal Due:       2025-06-01
Insurance:         Active (expires 2024-12-31)
Pollution Cert:    Active (expires 2024-12-15)

DECODED INFORMATION
State Code:       DL = Delhi
Priority Code:    01 = High priority registration
Series Letter:    A = 1st letter after state code
Series Letter:    B = 2nd letter after state code
Sequential No:    1234 = Vehicle number in series

SIMILAR VEHICLES
├─ DL-01-AC-1234 (Previous series)
├─ DL-01-AB-1233 (Previous number)
└─ DL-01-AB-1235 (Next number)

OWNER INFORMATION
Note: Detailed owner information requires access to
Parivahan database (restricted to authorized agencies)
```

---

### Module 6: Social Media / Username OSINT

**Purpose**: Find accounts across platforms using username

**Use Cases**:
- Does this person have accounts on multiple platforms?
- What's their presence across internet?
- What information do they publicly share?

**Tools Run**:
```
sherlock       → Cross-platform username search (300+ sites)
maigret        → Extended platform search
GitHub API     → Developer profile details
Platform checks → Direct website searches
```

**Example Output**:
```
USERNAME OSINT REPORT: john.doe
═════════════════════════════════════════════════════════

SEARCH RESULTS

FOUND ON THESE PLATFORMS:

GitHub
├─ URL:           https://github.com/john.doe
├─ Public Repos:  15
├─ Followers:     234
├─ Following:     89
├─ Public Gists:  5
└─ Latest:        Updated 2024-06-02

GitHub Repositories:
├─ web-framework (600 stars, 120 forks)
├─ nodejs-utils (450 stars, 95 forks)
├─ security-tools (200 stars, 50 forks)
└─ ... and 12 others

Twitter
├─ Handle:        @john.doe
├─ URL:           https://twitter.com/john.doe
├─ Followers:     1,200
├─ Following:     450
├─ Tweets:        3,456
├─ Verified:      No
└─ Last Tweet:    2024-06-04 14:23

LinkedIn
├─ Name:          John Doe
├─ URL:           https://linkedin.com/in/john-doe
├─ Title:         Senior Software Engineer
├─ Company:       Tech Corp Inc
├─ Connections:   500+
└─ Updated:       2024-06-02

Reddit
├─ Username:      u/john.doe
├─ URL:           https://reddit.com/u/john.doe
├─ Karma:         5,000+
├─ Posts:         342
├─ Comments:      1,200
└─ Active Subreddits: r/programming, r/webdev, r/security

Medium
├─ URL:           https://medium.com/@john.doe
├─ Articles:      12
├─ Followers:     234
├─ Reading List:  450
└─ Latest:        "Building Secure APIs" (2024-06-01)

Instagram
├─ Username:      john.doe.dev
├─ Followers:     482
├─ Following:     320
├─ Posts:         123
├─ Private:       No

Slack
├─ Workspace:     TechCorp
├─ Status:        Active
└─ Role:          Senior Developer

NOT FOUND ON:
├─ TikTok
├─ Snapchat
├─ WeChat
└─ WhatsApp (as expected - not searchable)

INTELLIGENCE SUMMARY
├─ Primary focus: Software development
├─ Active on professional platforms (GitHub, LinkedIn)
├─ Active on tech communities (Reddit, Stack Overflow)
├─ Social media presence (Twitter, Instagram) moderate
├─ Appears to be legitimate software engineer
└─ Last activity: 2-3 days ago
```

---

### Module 7: File/Metadata OSINT

**Purpose**: Extract hidden data from files

**Use Cases**:
- What's the creation date of this photo?
- Where was this photo taken (GPS)?
- What camera was used?
- Who created this document?
- Are there hidden comments/metadata?

**Tools Run**:
```
exiftool    → Metadata extraction
strings     → Hidden text/URLs
binwalk     → File structure analysis
pdfinfo     → PDF metadata
mediainfo   → Video/audio details
```

**Example Output**:
```
FILE OSINT REPORT: suspicious_photo.jpg
═════════════════════════════════════════════════════════

FILE INFORMATION
Filename:         suspicious_photo.jpg
File Size:        3.2 MB
File Type:        JPEG Image
Image Dimensions: 4000 x 3000 pixels
Color Space:      sRGB
Aspect Ratio:     4:3

EXIF METADATA (Image Properties)

Camera Information:
├─ Model:         Canon EOS 5D Mark III
├─ Manufacturer:  Canon Inc.
├─ Make:          Canon

Lens Information:
├─ Focal Length:  50.0 mm
├─ F-Number:      f/2.8
├─ Max Aperture:  f/2.8

Exposure Settings:
├─ ISO Speed:     400
├─ Shutter Speed: 1/320 second
├─ Exposure Bias: 0.67
├─ Metering Mode: Spot

Date/Time Information:
├─ Date Taken:    2024-06-15 14:23:45
├─ Timezone:      UTC
├─ Date Modified: 2024-06-20
└─ Date Created:  2024-06-15

GPS INFORMATION (CRITICAL!)
├─ GPS Latitude:   28.6139° N (28° 36' 50" N)
├─ GPS Longitude:  77.2090° E (77° 12' 32" E)
├─ GPS Altitude:   200 meters
├─ Location:       New Delhi, India
└─ Map Link:       https://maps.google.com/?q=28.6139,77.2090

Processing Information:
├─ Software:      Adobe Lightroom 5.2
├─ Processing:    Modified with filters
├─ Color Profile: sRGB
└─ Compressed:    No

EMBEDDED CONTENT
Thumbnails:       2 (one 160x120, one 320x240)
Color Space:      sRGB
XMP Metadata:     Yes (XMP profile embedded)

HIDDEN DATA ANALYSIS
Strings Found:    "Meeting location alpha"
                  "Do not share"
                  "Contact: john@gmail.com"

Suspicious Markers: Yes
├─ Metadata edited after creation
├─ GPS data present (privacy concern)
└─ Human-readable strings in binary

INTELLIGENCE SUMMARY
[!] Photo taken: 2024-06-15 14:23:45
[!] Location: New Delhi, India (exact coordinates)
[!] Camera: Professional Canon camera
[!] Hidden text: "Meeting location alpha"
[!] Contact info: john@gmail.com
[!] Privacy Concern: Metadata not stripped

RECOMMENDATIONS
- GPS data reveals precise location
- Metadata reveals equipment used
- Hidden strings indicate deliberate communication
- Photo was professionally edited
```

---

## Architecture

### System Design

```
┌────────────────────────────────────────┐
│     User (Investigator)                │
│   "python3 osint.py"                   │
└────────────────┬───────────────────────┘
                 │
         ┌───────▼──────────┐
         │  Main Menu Loop  │
         │ (osint.py)       │
         └───────┬──────────┘
                 │
    ┌────────────┼────────────┐
    │            │            │
    ▼            ▼            ▼
  Domain      IP OSINT    Email OSINT
  OSINT       Module      Module
  Module      │           │
  │           │           │
  ├─ whois    ├─ whois    ├─ dig
  ├─ dig      ├─ geoip    ├─ whois
  ├─ nmap     ├─ nmap     ├─ holehe
  ├─ dnsrecon ├─ tracert  ├─ sherlock
  │ ...       │ ...       │ ...
  │           │           │
  └─────────────┬──────────┘
                │
        ┌───────▼────────┐
        │  helpers.py    │
        │ (Tool Runner)  │
        └───────┬────────┘
                │
    ┌───────────┼────────────┐
    │           │            │
    ▼           ▼            ▼
 Execute    Parse       Format
  Tools    Output      Results
    │           │          │
    │      ┌────▼──────┐   │
    │      │ Validators│   │
    │      └────┬──────┘   │
    │           │          │
    └───────────┼──────────┘
                │
        ┌───────▼────────────┐
        │  API Enrichment    │
        │ (Optional)         │
        │ VirusTotal/Shodan  │
        └───────┬────────────┘
                │
        ┌───────▼────────────┐
        │ Reporter Module    │
        │ (Generate Reports) │
        └───────┬────────────┘
                │
    ┌───────────┼────────────┐
    │           │            │
    ▼           ▼            ▼
  .txt       .json       Console
 Report     Report      Output
```

### Module Structure

```
osint_framework/
│
├── osint.py                 # Main entry point
│   └── Menu-driven interface
│
├── config/
│   ├── settings.py          # Configuration management
│   │   └── API key handling
│   └── api_keys.json        # Stores API credentials (encrypted)
│
├── modules/
│   ├── domain_osint.py      # Domain investigation
│   ├── ip_osint.py          # IP investigation
│   ├── email_osint.py       # Email investigation
│   ├── phone_osint.py       # Phone investigation
│   ├── vehicle_osint.py     # Vehicle lookup
│   ├── social_osint.py      # Username search
│   └── file_osint.py        # Metadata extraction
│
├── utils/
│   ├── helpers.py           # Tool execution
│   │   ├── execute_command()
│   │   ├── parse_output()
│   │   └── format_results()
│   │
│   └── validators.py        # Input validation
│       ├── validate_domain()
│       ├── validate_ip()
│       └── validate_email()
│
└── reports/
    ├── domain_osint_2024-06-05.txt
    ├── domain_osint_2024-06-05.json
    ├── ip_osint_2024-06-03.txt
    └── ... (all generated reports)
```

---

## Technology Stack

### Core Technologies

```python
# Language
Python 3.8+

# HTTP Requests
requests              # API calls to external services

# Data Processing
pandas               # Organize results
json                # JSON report generation

# Subprocess Management
subprocess           # Run external tools safely
shlex               # Proper command parsing (prevent injection)

# Logging
logging              # Track operations and errors

# Configuration
python-dotenv        # Load environment variables
configparser        # Settings management

# Optional: API Libraries
virustotal          # VirusTotal API
shodan              # Shodan API
ipaddress           # IP validation
email-validator     # Email validation
```

### External Tools (Installed via setup.sh)

```bash
# DNS Tools
whois               # Domain registration lookup
dig                 # DNS queries
host                # Name resolution
dnsenum             # DNS enumeration
dnsrecon            # Advanced DNS

# IP Tools
geoiplookup         # Geolocation
ipcalc              # Subnet math
traceroute          # Network path
nmap                # Port scanning

# Web Tools
whatweb             # Technology identification
sslscan             # SSL analysis

# OSINT Tools
amass               # Subdomain enumeration
curl                # HTTP requests

# File Tools
exiftool            # Metadata extraction
strings             # Extract text from binaries
binwalk             # File structure analysis
```

---

## API Integration

### Optional APIs (All Optional - Framework Works Without Them)

The framework has **graceful degradation** - if you don't have API keys, it still works with command-line tools.

| API | Use Case | Cost | When Used |
|-----|----------|------|-----------|
| **VirusTotal** | File/URL/IP reputation | Free (75 req/min) | Domain/IP scanning |
| **Shodan** | Service enumeration | Free (1/min), Paid | IP intelligence |
| **IPInfo** | IP geolocation | Free (50k/month) | IP location accuracy |
| **AbuseIPDB** | Abuse reports | Free (15k/day) | IP abuse scoring |
| **HaveIBeenPwned** | Breach checking | Free | Email breach checking |
| **Hunter.io** | Email finding | Free (10/day) | Email verification |
| **NumVerify** | Phone validation | Free (100/month) | Phone number validation |
| **Twilio** | Carrier lookup | Paid | Carrier identification |
| **GitHub** | Dev profiles | Free | Developer information |

### Configuration Methods

**Method 1: Interactive Menu**
```
python3 osint.py
  → Option 9: Configure API Keys
  → Select API to configure
  → Enter your API key
  → Save to api_keys.json
```

**Method 2: Environment Variables**
```bash
export VT_API_KEY="your_virustotal_key"
export SHODAN_API_KEY="your_shodan_key"
export IPINFO_TOKEN="your_ipinfo_token"
# ... etc

python3 osint.py  # Framework reads from env
```

**Method 3: Direct File Edit**
```json
{
  "virustotal": "your_key_here",
  "shodan": "your_key_here",
  "ipinfo": "your_key_here",
  "abuseipdb": "your_key_here",
  "haveibeenpwned": "your_key_here",
  "hunter_io": "your_key_here",
  "numverify": "your_key_here",
  "twilio_sid": "your_sid",
  "twilio_token": "your_token",
  "github": "your_token"
}
```

### Fallback Behavior

When API keys are missing:

```python
# VirusTotal missing?
# Instead of API call, just use command-line tools

try:
    result = virustotal_api.check(domain)
except API_KEY_MISSING:
    logger.warning("VirusTotal API not configured, skipping")
    # Continue with nmap, whatweb, sslscan results
```

---

## Safety & Legal Compliance

### Passive Methods (No Permission Needed)

These are COMPLETELY SAFE and LEGAL:
```
✓ WHOIS lookups          (Public database)
✓ DNS queries            (Public DNS)
✓ Certificate checking   (Public certificates)
✓ Web searches           (Public information)
✓ API queries            (Public APIs)
✓ Social media profile   (Public profiles)
✓ File metadata          (Your own files)
✓ String searches        (Text content)
```

### Active Methods (Require Confirmation)

These require explicit user permission:
```
⚠ Port scanning (nmap)   - Can be detected
⚠ Network probing        - May trigger alerts
⚠ Service enumeration    - Could impact systems
```

When user tries active method:
```
⚠ WARNING: This will perform ACTIVE scanning
⚠ This can be detected by IDS systems
⚠ May trigger security alerts
⚠ Should only be done with proper authorization
⚠ Are you authorized? [Y/N]
```

### Illegal Methods (Completely Prohibited)

The framework NEVER:
```
✗ Attempts unauthorized network access
✗ Tries to break passwords
✗ Accesses emails without permission
✗ Exploits vulnerabilities
✗ Performs DDoS attacks
✗ Installs malware
✗ Accesses phone/SIM systems
✗ Intercepts communications
```

### Input Sanitization

All user input is validated against injection attacks:

```python
# User input: "suspicious.com; rm -rf /"
domain = input("Enter domain: ")

# Validate format
if not is_valid_domain(domain):
    raise InvalidDomainError("Invalid domain format")

# Safe execution
subprocess.run(['whois', domain], shell=False)
# Not vulnerable because shell=False prevents injection
```

### Legal Compliance Measures

```python
# 1. Operator Audit Log
logger.info(f"User {operator_id} investigated {target}")
logger.info(f"Method: {investigation_type}")
logger.info(f"Timestamp: {datetime.now()}")

# 2. Rate Limiting (Prevent abuse)
if requests_in_last_hour > 100:
    raise RateLimitError("Too many requests")

# 3. Target Validation
if is_honeypot_domain(domain):
    logger.warning("Honeypot detected!")
    
# 4. Disclaimer on startup
print("╔════════════════════════════════════════════╗")
print("║  OSINT Investigation Framework             ║")
print("║  FOR AUTHORIZED LAW ENFORCEMENT ONLY       ║")
print("║  Misuse of this tool is a crime            ║")
print("╚════════════════════════════════════════════╝")
```

---

## Output Format

### Report Structure

Every investigation produces 2 formats automatically:

#### **Format 1: Text Report (.txt)**

```
═════════════════════════════════════════════════════
OSINT INVESTIGATION REPORT
Target: example.com
Investigation Type: Domain OSINT
Date: 2024-06-05 14:23:45
═════════════════════════════════════════════════════

SECTION 1: TOOL EXECUTION SUMMARY
────────────────────────────────────────────────────
✓ whois           - Success (completed in 2.34s)
✓ dig             - Success (completed in 1.23s)
✓ host            - Success (completed in 0.56s)
✓ dnsrecon        - Success (completed in 5.67s)
⚠ nmap            - Skipped (not confirmed by user)
✗ traceroute      - Failed (timeout after 10s)
✓ whatweb         - Success (completed in 3.45s)

Summary: 6/7 tools successful, 1 skipped, 1 failed

SECTION 2: RAW TOOL OUTPUT
────────────────────────────────────────────────────
[Full unmodified output from each tool, separated by tool name]

SECTION 3: PARSED & VERIFIED DATA
────────────────────────────────────────────────────
Registrant:       Example Corp Inc
Created:          2010-01-15
Expires:          2025-01-15
IP Address:       93.184.216.34
Country:          United States
...

SECTION 4: CROSS-VERIFICATION STATUS
────────────────────────────────────────────────────
IP Verification:
├─ whois says IP is 93.184.216.34 ✓
├─ dig A record is 93.184.216.34 ✓
├─ host resolution confirms 93.184.216.34 ✓
└─ All sources agree ✓ VERIFIED

Registrant Verification:
├─ whois says registrant is "Example Corp" ✓
├─ ICANN WHOIS says "Example Corp Inc" (minor variation) ⚠
└─ Likely same entity ✓ VERIFIED

SECTION 5: LIMITATIONS & NOTES
────────────────────────────────────────────────────
[What we know, what we don't, and why]

- WHOIS information may be privacy-protected
- IP geolocation accurate to city level (±5km)
- Could not determine exact server software versions
- Optional API (Shodan) not configured
- nmap not confirmed by user (passive analysis only)
```

#### **Format 2: JSON Report (.json)**

```json
{
  "metadata": {
    "version": "3.1",
    "target": "example.com",
    "investigation_type": "domain_osint",
    "timestamp": "2024-06-05T14:23:45Z",
    "operator_id": "operator_123",
    "duration_seconds": 35.6
  },
  
  "tool_results": {
    "whois": {
      "status": "success",
      "duration_ms": 2340,
      "output": {...},
      "parsed": {
        "registrant": "Example Corp Inc",
        "created": "2010-01-15",
        "expires": "2025-01-15"
      }
    },
    "dig": {
      "status": "success",
      "duration_ms": 1230,
      "output": {...},
      "parsed": {
        "a_record": "93.184.216.34",
        "mx_record": "mail.example.com",
        "ns_records": ["ns1.example.com", "ns2.example.com"]
      }
    },
    "nmap": {
      "status": "skipped",
      "reason": "not_confirmed_by_user"
    },
    "traceroute": {
      "status": "failed",
      "error_message": "Timeout after 10 seconds"
    }
  },
  
  "correlated_intelligence": {
    "domain": {
      "name": "example.com",
      "registrant": "Example Corp Inc",
      "created": "2010-01-15",
      "expires": "2025-01-15",
      "registrar": "VeriSign Global Registry Services"
    },
    "ip_address": {
      "address": "93.184.216.34",
      "organization": "Edgecast Networks Inc",
      "country": "United States",
      "city": "Los Angeles",
      "coordinates": {
        "latitude": 34.0544,
        "longitude": -118.2439
      }
    },
    "dns_records": {
      "a_record": "93.184.216.34",
      "mx_record": ["mail.example.com", "mail2.example.com"],
      "ns_record": ["ns1.example.com", "ns2.example.com"],
      "spf_record": "v=spf1 include:_spf.google.com ~all"
    },
    "services": {
      "port_80": "HTTP - Apache/2.4.41",
      "port_443": "HTTPS - Certificate valid"
    }
  },
  
  "verification_status": {
    "ip_address_verified": true,
    "registrant_verified": true,
    "dns_records_verified": true,
    "conflicts": []
  },
  
  "api_enrichment": {
    "virustotal": {
      "configured": false
    },
    "shodan": {
      "configured": true,
      "status": "success",
      "data": {...}
    }
  },
  
  "limitations": [
    "WHOIS information may be privacy-protected",
    "IP geolocation accurate to city level",
    "Cannot determine exact server versions without Shodan API",
    "Port scanning skipped (passive analysis only)"
  ]
}
```

---

## Interview Q&A

### Basic Questions

**Q1: What problem does this project solve?**

A: OSINT investigators manually running 20+ different tools and compiling results is time-consuming and error-prone. My framework automates this:
- Single command instead of 20+ manual commands
- Consistent output format instead of learning each tool's syntax
- Automatic correlation across sources
- Professional report generation
- Reduces 2-3 hours of work to 5-10 minutes

---

**Q2: Why Python for this project?**

A: Python is ideal for automation and tooling:
- Easy to call external commands (subprocess)
- Great libraries for parsing output (regex, string processing)
- Simple CLI interface (no complex UI needed)
- Fast development for investigators who need tools quickly
- Large OSINT community already uses Python

---

**Q3: How do you handle different tool output formats?**

A: Each module has custom parsing logic:

```python
# whois output is line-based key-value pairs
def parse_whois(output):
    result = {}
    for line in output.split('\n'):
        if ':' in line:
            key, value = line.split(':', 1)
            result[key.strip()] = value.strip()
    return result

# dig output is structured with sections
def parse_dig(output):
    result = {'answers': []}
    in_answer = False
    for line in output.split('\n'):
        if 'ANSWER SECTION' in line:
            in_answer = True
        elif in_answer:
            result['answers'].append(parse_dns_line(line))
    return result

# Each tool has its own parser
```

---

**Q4: Why are API keys optional?**

A: Several reasons:
1. **Cost**: APIs cost money, not everyone has budgets
2. **Accessibility**: Investigators in smaller agencies should still get value
3. **Privacy**: Some investigators don't want to send data to external APIs
4. **Graceful Degradation**: Command-line tools still work
5. **Flexibility**: Advanced users add APIs, beginners start without

The framework detects available keys and uses APIs when present, falls back to tools when not.

---

**Q5: How do you ensure accuracy of results?**

A: Cross-verification across multiple sources:

```python
# If whois, dig, and host all agree on IP → VERIFIED
# If sources disagree → FLAG as CONFLICT
# If source doesn't have answer → MARK as UNKNOWN

verification = {
    'ip_address': {
        'whois': '93.184.216.34',
        'dig': '93.184.216.34',
        'host': '93.184.216.34',
        'consensus': 'VERIFIED'
    },
    'registrant': {
        'whois': 'Example Corp Inc',
        'icann_whois': 'EXAMPLE CORP (different case)',
        'consensus': 'LIKELY SAME'
    }
}
```

---

**Q6: What's the biggest challenge in OSINT automation?**

A: **Keeping up with changing websites and APIs**. Websites change their HTML structure, APIs change endpoints, tools add new options. My solution:
1. Modular design (update one parser without breaking others)
2. Error handling (if one source fails, others continue)
3. Community contributions (others improve parsers)
4. Version control (track changes)

---

### Technical Questions

**Q7: How do you prevent command injection?**

A: Using `subprocess.run()` with `shell=False`:

```python
# SAFE (shell=False)
subprocess.run(['whois', domain], shell=False)
# No matter what domain is, it's just an argument

# UNSAFE (shell=True) - DO NOT USE
subprocess.run(f'whois {domain}', shell=True)
# If domain = "example.com; rm -rf /"
# The rm -rf command would execute!
```

Never use `shell=True` with user input.

---

**Q8: How do you validate input without being too restrictive?**

A: I use specific validators but allow some flexibility:

```python
# Domain validation
def validate_domain(domain):
    # Check length
    if len(domain) < 3 or len(domain) > 255:
        raise InvalidDomainError("Domain too short/long")
    
    # Check characters
    if not re.match(r'^[a-zA-Z0-9.-]+$', domain):
        raise InvalidDomainError("Invalid characters")
    
    # Check format (must have TLD)
    if not re.match(r'^[\w.-]+\.\w+$', domain):
        raise InvalidDomainError("Must have TLD (e.g., .com)")
    
    return domain

# This allows:
# ✓ example.com
# ✓ sub.example.co.uk
# ✓ test-domain.info
# But blocks:
# ✗ example.com; ls
# ✗ ../../../etc/passwd
```

---

**Q9: How do you handle rate limiting?**

A: Several approaches:

```python
# 1. Tool-level rate limiting
import time
def run_whois(domain):
    if last_whois_run < 2 seconds ago:
        time.sleep(2)  # Wait 2 seconds between whois calls
    
# 2. API-level rate limiting
from ratelimit import limits, sleep_and_retry

@sleep_and_retry
@limits(calls=1, period=1)  # Max 1 call per second
def call_shodan_api(ip):
    return api.host(ip)

# 3. Overall limits
def run_investigation(target):
    if investigations_in_last_hour > 100:
        raise RateLimitError("Too many investigations")
```

---

**Q10: How do you report errors without losing context?**

A: Detailed logging at every step:

```python
import logging

logger = logging.getLogger(__name__)

try:
    result = subprocess.run(
        ['whois', domain],
        capture_output=True,
        timeout=10
    )
    logger.info(f"whois successful for {domain}")
    
except subprocess.TimeoutExpired:
    logger.error(f"whois timeout for {domain} (exceeded 10s)")
    results['whois'] = {
        'status': 'failed',
        'error': 'timeout',
        'details': 'Whois server not responding'
    }
    
except FileNotFoundError:
    logger.error(f"whois not installed")
    results['whois'] = {
        'status': 'failed',
        'error': 'tool_not_found',
        'details': 'Run setup.sh to install whois'
    }

# Report what failed and why (user can make informed decisions)
```

---

## Interview Story

**Tell this when asked about your biggest project:**

> "I built an OSINT Investigation Framework for law enforcement to automate intelligence gathering.
>
> **The Problem**: Investigators manually running 20+ different tools (whois, dig, nmap, sslscan, etc.), each with different output formats, then manually compiling results into reports. This took 2-3 hours per investigation.
>
> **My Solution**: A unified Python framework that:
> 1. Runs all relevant tools automatically based on target type
> 2. Parses different tool outputs into consistent format
> 3. Cross-verifies information across sources
> 4. Optional API enrichment (VirusTotal, Shodan, etc.)
> 5. Generates professional TXT + JSON reports
>
> **Key Technical Challenges**:
> - **Output Parsing**: Each tool has different format (whois = key-value, dig = DNS sections, nmap = tables). Had to build custom parsers.
> - **Tool Orchestration**: Running 20 tools concurrently, handling failures gracefully.
> - **Security**: Ensuring subprocess calls can't be injection attacked using shell=False.
> - **API Flexibility**: Making APIs optional so framework works without them.
> - **Legal Compliance**: Passive methods only by default, active methods require user confirmation.
>
> **Results**:
> - 10-minute investigations instead of 2-3 hours
> - Consistent, professional reports
> - Investigators focus on analysis, not data collection
> - Zero injection vulnerabilities
> - Used by law enforcement agencies
>
> This project taught me:
> - Subprocess management in Python
> - Regular expression parsing
> - API integration
> - Security best practices
> - Tool design for investigators"

---

## Key Points to Remember

1. **Automation**: Removes manual work from OSINT
2. **Modular**: Add new modules without breaking existing ones
3. **Optional APIs**: Works without API keys (graceful degradation)
4. **Safe**: Passive-only by default, active methods require confirmation
5. **Professional**: Generates investigation-ready reports
6. **Cross-verified**: Checks information across multiple sources
7. **Well-logged**: Every operation logged for audit trails

---

## Numbers to Mention

- **7 investigation modules** (Domain, IP, Email, Phone, Vehicle, Username, File)
- **20+ integrated tools** (whois, dig, nmap, etc.)
- **9 optional APIs** (VirusTotal, Shodan, etc.)
- **2 output formats** (TXT + JSON)
- **5 report sections** (Summary, Raw, Parsed, Verified, Limitations)
- **1 command** to complete investigation
- **10 minutes** average investigation time
- **100+ platforms** for username search (sherlock database)
- **~2-3 hours saved** per investigation

Good luck! 🎯
