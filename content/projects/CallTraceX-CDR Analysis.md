---
title: 'CDR Forensic Analysis'
date: 2026-06-05
draft: false
tags: ["forensics", "metadata", "python"]
---


> **A desktop application for law enforcement that automates forensic analysis of Call Detail Records from Indian telecom providers**

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [The Problem It Solves](#the-problem-it-solves)
3. [Key Features](#key-features)
4. [Technology Stack](#technology-stack)
5. [Architecture](#architecture)
6. [Investigation Modules](#investigation-modules)
7. [Real-World Use Cases](#real-world-use-cases)
8. [Interview Q&A](#interview-qa)

---

## Project Overview

The **CDR Forensic Analysis Tool** is a desktop application designed for law enforcement agencies, cybercrime investigators, and digital forensic professionals. It automates the analysis of **Call Detail Records (CDRs)** obtained from Indian telecom service providers.

Instead of manually analyzing thousands of phone records, this tool:
- **Parses** data automatically (detects operator format)
- **Cleans** invalid/corrupted records
- **Analyzes** from 20+ different angles
- **Generates** professional Excel reports in minutes

### Quick Stats
- **Language**: Python
- **GUI Framework**: Tkinter
- **Data Processing**: Pandas + NumPy
- **Output**: Excel Reports (OpenPyXL)
- **Target Users**: Law enforcement, cybercrime units
- **Supported Operators**: Airtel, Jio, Vodafone Idea (Vi), BSNL

---

## The Problem It Solves

### Scenario: A Typical Investigation

A cybercrime investigator receives CDR data from a telecom provider. The challenge:

```
❌ BEFORE Tool:
├─ Received 50,000 CDR entries in CSV format
├─ Different columns than other operators
├─ Manual data cleaning: 3-4 hours
├─ Contact correlation: 5-6 hours
├─ Location analysis: 4-5 hours
├─ Device tracking: 3-4 hours
├─ Report generation: 2-3 hours
└─ TOTAL TIME: 18-25 hours ⏱️

✅ WITH This Tool:
├─ Load file: 2 minutes
├─ Auto-parse & clean: 3 minutes
├─ Auto-analyze (20+ sheets): 5 minutes
├─ Report generated: 1 minute
└─ TOTAL TIME: 11 minutes ⚡
```

### Major Challenges Addressed

1. **Different Telecom Formats**
   - Airtel uses columns: A_PARTY, B_PARTY, DURATION
   - Jio uses: CALLED_PARTY, CALLING_PARTY, CALL_LENGTH
   - Vi uses: CALLED_NUMBER, CALLER_NUMBER, DURATION_SECS
   - BSNL uses: INCOMING_CALLS, OUTGOING_CALLS, LENGTH
   - **Solution**: Automatic format detection and mapping

2. **Data Quality Issues**
   - Missing values in critical fields
   - Incorrect date formats
   - Encoding problems (ISO-8859-1 vs UTF-8)
   - Invalid phone numbers
   - **Solution**: Intelligent cleaning with validation

3. **Large Volume Processing**
   - 50,000+ records in single file
   - Processing becomes slow with Python loops
   - **Solution**: Pandas vectorized operations (100x faster)

4. **Manual Correlation**
   - Manually finding all contacts
   - Calculating call durations
   - Identifying devices
   - **Solution**: Automated aggregation and grouping

---

## Key Features

### 1. Multi-Operator Support

```
Airtel ────┐
Jio ────┐  │
Vi ──┐  │  ├──> Parser ──> Standardized Format
BSNL─┘  │  │
        └──┘
```

The parser automatically detects which operator the data is from by analyzing:
- Column names and order
- Data format patterns
- Metadata headers
- File structure

**No manual preprocessing needed** - just load the file!

### 2. Automatic Metadata Extraction

Before analysis begins, the tool extracts:
- ✓ Subscriber name and mobile number
- ✓ IMSI (SIM identifier)
- ✓ IMEI (Device identifier)
- ✓ Circle/Region information
- ✓ Ticket number
- ✓ SIM activation date
- ✓ Address details
- ✓ Record date range
- ✓ Telecom operator name

### 3. Intelligent Data Cleaning

```python
Raw Data Issues          →  Cleaned Data
─────────────────────────────────────────
9876543210             →  +91-9876543210
(Invalid numbers)      →  (Removed)
2024-01-15 (mixed)     →  2024-01-15
2024/01/15 (mixed)     →  2024-01-15
UTF-8 + ISO mix        →  UTF-8 (consistent)
NULL values            →  Handled intelligently
```

Process:
- Removes completely invalid records
- Handles missing data intelligently
- Standardizes phone number format
- Normalizes all date/time formats
- Detects and fixes encoding issues
- Converts all columns to consistent types

### 4. Comprehensive Analysis (20+ Investigation Sheets)

#### **Contact Analysis**
| Sheet | What It Shows | Use Case |
|-------|---------------|----------|
| Contact Summary | All unique contacts with interaction count | Identify important associates |
| Top Callers | Who called the target most | Find frequent callers |
| Top Called Numbers | Whom the target called most | Find frequent contacts |
| Contact Duration Summary | Total talk time per contact | Identify closest relationships |

#### **Device Analysis**
| Sheet | What It Shows | Use Case |
|-------|---------------|----------|
| IMEI Summary | All devices used with dates | Device tracking |
| IMSI Summary | All SIM cards used | SIM history |
| Common IMEI Detection | Multiple numbers on same device | Shared device identification |

#### **Location Analysis**
| Sheet | What It Shows | Use Case |
|-------|---------------|----------|
| Location Summary | Frequently used cell towers | Geographical presence |
| Cell Tower Analysis | Detailed location coordinates | Place suspect at location |
| Roaming Summary | Travel across regions | Movement patterns |

#### **Special Feature: Day & Night Halt Analysis**

**Day Halt Analysis**
- Identifies where the target stays during business hours
- Shows frequently used cell towers between 9 AM - 6 PM
- **Investigation Value**: Pinpoints work location or regular activity zone

**Night Halt Analysis**
- Identifies where the target stays at night
- Shows frequently used cell towers between 10 PM - 6 AM
- **Investigation Value**: Can identify home address, safe houses, hideouts

```
Example Output:
Day Halt Location:
├─ Tower ID: DLH_SECTOR_15
├─ Activity: 156 records (highest)
├─ Hours: 9 AM - 6 PM
└─ Interpretation: Likely workplace in Delhi Sector 15

Night Halt Location:
├─ Tower ID: DLH_DWARKA_04
├─ Activity: 142 records (highest)
├─ Hours: 10 PM - 6 AM
└─ Interpretation: Likely home address in Dwarka, Delhi
```

#### **Temporal Analysis**
| Sheet | What It Shows |
|-------|---------------|
| Hourly Activity | When is target most active? |
| Weekday Analysis | Activity patterns by day |
| Call Hold Detection | Suspicious communication patterns |
| Conference Call Detection | Group communication identification |

#### **Behavioral Analysis**
| Sheet | What It Shows |
|-------|---------------|
| Communication Pattern | Call frequency trends |
| Roaming Pattern | Travel patterns over time |
| Device Switch Analysis | When devices were changed |

---

## Technology Stack

### Core Technologies
```
Python 3.x
├── Pandas          (data manipulation & cleaning)
├── NumPy           (numerical computations)
├── Tkinter         (GUI framework)
├── OpenPyXL        (Excel report generation)
└── Python Logging  (error tracking & debugging)
```

### Why These Technologies?

| Technology | Why Chosen | Alternative |
|-----------|-----------|-------------|
| Python | Fast development, large community | C++, Java |
| Pandas | Perfect for data cleaning | SQL, Spark |
| Tkinter | Simple desktop GUI | Qt, wxPython |
| OpenPyXL | Excel generation without Office | xlsxwriter |

---

## Architecture

### System Components

```
┌────────────────────────────────────────┐
│        GUI Module (Tkinter)            │
│  ┌──────────────────────────────────┐  │
│  │ File Selection Interface         │  │
│  │ Progress Bar & Real-time Logs    │  │
│  │ Report Folder Access             │  │
│  └──────────────────────────────────┘  │
└────────────────┬───────────────────────┘
                 │ CSV File
         ┌───────▼────────┐
         │ Parser Module  │
         └───────┬────────┘
                 │
    ┌─ Operator Detection
    ├─ Metadata Extraction
    ├─ Encoding Detection
    ├─ Column Mapping
    └─ Data Cleaning
                 │
            DataFrame
                 │
         ┌───────▼─────────┐
         │ Analyzer Module │
         └───────┬─────────┘
                 │
    ├─ Contact Analysis
    ├─ Device Correlation
    ├─ Location Intelligence
    ├─ Roaming Analysis
    ├─ Halt Pattern Detection
    ├─ Temporal Analysis
    └─ Behavioral Patterns
                 │
        Multiple DataFrames
                 │
         ┌───────▼──────────┐
         │ Reporter Module  │
         └───────┬──────────┘
                 │
    ├─ Multi-sheet Excel
    ├─ Formatting
    ├─ Statistics
    └─ Investigation Summary
                 │
            Excel Report
```

### Data Flow

```
Step 1: Load CDR File
   │
   └─> CSV Reader
        └─> Detect Encoding
             └─> Load into Memory

Step 2: Parse & Clean
   │
   ├─> Operator Detection
   │    └─> Apply Correct Format Mapping
   ├─> Data Validation
   │    └─> Remove Invalid Records
   ├─> Standardization
   │    └─> Normalize Formats
   └─> Quality Check
        └─> Report Cleaning Statistics

Step 3: Analyze
   │
   ├─> Group by Contact
   │    └─> Calculate Duration, Frequency
   ├─> Group by IMEI
   │    └─> Device Timeline Analysis
   ├─> Group by Cell Tower
   │    └─> Location & Roaming Analysis
   ├─> Group by Time
   │    └─> Activity Patterns
   └─> Detect Special Patterns
        └─> Halt, Hold, Conference Calls

Step 4: Generate Report
   │
   └─> Excel Writer
        ├─> Sheet 1: Summary
        ├─> Sheets 2-21: Analysis Results
        ├─> Format & Styling
        └─> Save Report
```

---

## Investigation Modules

### Module 1: Contact Analysis

**Purpose**: Understand communication network

**Process**:
```python
# Pseudocode of what happens
for each_unique_number in data:
    count_interactions()
    calculate_total_duration()
    find_first_last_date()
    calculate_frequency()
    store_in_contact_summary()

sort_by_frequency()
generate_contact_sheet()
```

**Output Example**:
```
Contact Summary
═════════════════════════════════════════════
Contact Number    | Count | Duration | Days
─────────────────────────────────────────────
9876543210       | 156   | 3245 min | 47
9765432109       | 123   | 1987 min | 35
9654321098       | 98    | 1234 min | 28
9543210987       | 67    | 890 min  | 19
─────────────────────────────────────────────
```

### Module 2: Device Analysis (IMEI/IMSI)

**Purpose**: Track which devices/SIMs were used

**IMEI** = Device Identifier (hardware)
**IMSI** = SIM Card Identifier (software)

**Investigation Value**:
- If suspect uses multiple SIM cards on one device → Controlled activity
- If suspect uses one SIM on multiple devices → Shared responsibility
- IMEI changes indicate device swap → Attempts to hide identity

**Output Example**:
```
IMEI Summary
═════════════════════════════════════════════
IMEI            | Count | Device Type | Dates
─────────────────────────────────────────────
35201403000000  | 234   | iPhone 12   | 47 days
35219204000000  | 156   | Samsung S21 | 32 days
35203105000000  | 98    | Redmi Note  | 21 days
─────────────────────────────────────────────

Common IMEI Detection
═════════════════════════════════════════════
IMEI            | SIM Cards Used | Dates
─────────────────────────────────────────────
35201403000000  | 3 different    | 47 days
                | 9876543210     | Days 1-15
                | 9765432109     | Days 16-35
                | 9654321098     | Days 36-47
─────────────────────────────────────────────
```

### Module 3: Location Analysis

**Purpose**: Determine geographical presence

**How It Works**:
- Each cell tower has a specific coverage area
- When a call is made, the nearest tower is used
- Tower location = approximate user location
- Frequency of tower usage = time spent there

**Output Example**:
```
Location Summary (Top 10)
═════════════════════════════════════════════════════
Tower ID    | City      | Count | Coordinates
─────────────────────────────────────────────────────
DLH_15_001  | Delhi     | 234   | 28.5244, 77.0855
DLH_DWRK_04 | Delhi     | 189   | 28.5494, 77.0410
GGN_22_005  | Gurgaon   | 145   | 28.4595, 77.0266
DLH_15_002  | Delhi     | 128   | 28.5267, 77.0889
GGN_42_001  | Gurgaon   | 98    | 28.4524, 77.0433
─────────────────────────────────────────────────────
```

**Critical Insight: Day Halt vs Night Halt**

```
Day Halt Analysis:
├─ Time Window: 9 AM - 6 PM
├─ Tower: DLH_SECTOR_15_001 (234 records)
├─ Interpretation: WORKPLACE/OFFICE LOCATION
└─ Coordinates: 28.5244, 77.0855 (Use Google Maps)

Night Halt Analysis:
├─ Time Window: 10 PM - 6 AM
├─ Tower: DLH_DWARKA_04 (189 records)
├─ Interpretation: HOME ADDRESS / HIDEOUT
└─ Coordinates: 28.5494, 77.0410 (Verify with address database)
```

---

## Real-World Use Cases

### Case 1: Cybercrime Investigation (Online Fraud Ring)

**Scenario**: 
- Police received reports of an online scam
- They have CDR data of the main suspect's number
- Need to identify the criminal network

**Analysis Process**:
1. **Load CDR Data**
   - 30,000 records over 60 days

2. **Contact Analysis**
   - Identify 156 unique contacts
   - Ranking: Top 15 contacts are frequent

3. **Device Analysis**
   - Suspect used 3 different IMEI numbers
   - Indicates careful operational security
   - Common IMEI detection shows coordination with 5 other numbers

4. **Location Analysis**
   - Day Halt: Delhi Sector 15 (office address)
   - Night Halt: Dwarka (residential address)
   - Roaming: Weekly visit to Noida

5. **Behavioral Analysis**
   - Peak activity: 11 AM - 1 PM (coordinating with team)
   - Pattern: Daily visits to office location
   - Conference calls detected: 23 instances (group coordination)

**Outcome**:
- Map 45 associates
- Identify safe houses (multiple night halts)
- Establish daily routine
- Link to physical locations for surveillance

---

### Case 2: Missing Person

**Scenario**:
- 25-year-old student missing for 3 days
- Family has CDR data from telecom provider
- Need to find last known location and contacts

**Analysis Process**:
1. **Immediate Actions**
   - Load latest CDR (last 3 days = 500 records)
   - Check last tower activity

2. **Location Analysis**
   - Last activity: Tower at Noida City Center (8:45 PM)
   - Within 500m radius of location
   - Frame 2 hours before last activity

3. **Contact Analysis**
   - Last call: To unknown number (9876543210)
   - This number called 2 times before
   - Need to identify this number

4. **Timeline Construction**
   ```
   Day 1 (12 hrs data):
   ├─ 7:00 AM: Tower DLH_15 (home)
   ├─ 9:00 AM: Tower DLH_SECTOR_15 (office/college)
   ├─ 6:00 PM: Tower DLH_15 (returned home)
   └─ 8:00 PM: Tower DLH_15 (last activity here)
   
   Day 2: NO ACTIVITY (phone off or no signal)
   
   Day 3 (before going missing):
   ├─ 2:00 PM: Tower GGN_CITY_CENTER (shopping mall)
   ├─ 4:30 PM: Called unknown number
   ├─ 6:00 PM: Tower NOIDA_EXPRESSWAY
   ├─ 8:45 PM: Tower NOIDA_CITY_CENTER (LAST ACTIVITY)
   └─ After 8:45 PM: NO SIGNAL
   ```

**Outcome**:
- Pinpoint last location to 500m area
- Identify unknown number for investigation
- Timeline helps establish narrative
- Lead for search teams

---

### Case 3: Organized Crime Investigation

**Scenario**:
- Police investigating drug trafficking network
- Have CDR data of 5 suspected members
- Need to establish hierarchy and communication

**Analysis Process**:
1. **Individual Analysis**
   - Analyze each suspect's CDR separately
   - Identify personal network for each

2. **Network Mapping**
   - Cross-reference contacts
   - Find common numbers (coordinators)
   - Identify indirect connections

3. **Behavioral Patterns**
   - Who calls whom most?
   - Who initiates calls?
   - Time patterns (early bird vs night owl)

4. **Device Sharing**
   - IMEI analysis shows device sharing
   - Multiple SIMs on one device = coordination
   - Changing devices = security awareness

5. **Location Intelligence**
   - Meeting points identification (multiple day halts)
   - Supply chain locations (roaming patterns)
   - Safe houses (night halts)

**Outcome**:
- Establish hierarchy (frequent caller = boss)
- Identify communication protocol
- Locate key meeting points
- Determine supply routes

---

## Interview Q&A

### Basic Questions

**Q1: What problem does this tool solve?**

A: It automates forensic analysis of telecom CDRs. Instead of investigators manually analyzing thousands of records over days, this tool does it in minutes by automatically parsing, cleaning, and analyzing data from 4 different telecom providers.

---

**Q2: Why did you use Python?**

A: Python has excellent libraries for this use case:
- **Pandas**: Perfect for data manipulation and cleaning (100x faster than loops)
- **NumPy**: Efficient numerical operations
- **Tkinter**: Simple GUI without external dependencies
- **OpenPyXL**: Professional Excel report generation
- Fast development time for law enforcement needs

---

**Q3: How does the parser automatically detect the operator?**

A: The parser analyzes several features:
1. **Column Names**: Each operator uses different naming conventions
2. **Data Format**: Different date/number formats
3. **File Structure**: Metadata headers vary
4. **Sample Matching**: Try multiple format patterns, measure success rate

The parser tries each known operator's pattern and selects the one that successfully parses the most rows.

```python
# Pseudocode
for operator in [AIRTEL, JIO, VI, BSNL]:
    try:
        df = operator_parser.parse(file)
        success_rate = count_valid_rows(df) / total_rows
        if success_rate > 0.95:  # 95% threshold
            return operator
```

---

**Q4: What data cleaning steps do you perform?**

A: 
1. **Remove Invalid Records**
   - Records missing critical fields (number, date, duration)
   
2. **Standardize Phone Numbers**
   - Convert 9876543210 → +91-9876543210
   - Handle 10-digit and 11-digit formats
   
3. **Fix Date/Time Formats**
   - Convert various formats to single format (YYYY-MM-DD)
   - Handle mixed formats in same file
   
4. **Encoding Issues**
   - Detect and convert ISO-8859-1 to UTF-8
   - Handle special characters
   
5. **Data Type Conversion**
   - Convert string numbers to integers
   - Convert dates to datetime objects
   
6. **Handle Missing Values**
   - NULL/empty fields
   - Replace with sensible defaults or skip row
   
Report: "Processed 50000 records, cleaned 49500, removed 500 invalid"

---

**Q5: What is Day Halt and Night Halt analysis?**

A: 
- **Day Halt**: Identifies where target stays during 9 AM - 6 PM
  - Usually indicates workplace or regular activity location
  - Frequently used cell tower during these hours
  
- **Night Halt**: Identifies where target stays during 10 PM - 6 AM
  - Usually indicates home address or hideout
  - Frequently used cell tower during these hours

**Investigation Value**:
- Can place suspect at work location during specific time
- Can identify home address for verification
- Can identify hideouts in crime cases
- Helps establish routine patterns

---

**Q6: How do you handle very large datasets (100,000+ records)?**

A: Several optimization techniques:
1. **Pandas Vectorization**: Avoid Python loops
   ```python
   # SLOW (1000x slower)
   for row in df:
       total = sum(row)
   
   # FAST (Vectorized)
   df.sum()
   ```

2. **Chunked Processing**: Process in smaller batches
3. **Efficient Data Types**: Use category type for repeated values
4. **Strategic Filtering**: Filter before grouping
5. **Indexing**: Create indexes on frequently used columns

---

**Q7: What are the limitations of CDR analysis?**

A: Several important limitations:
1. **Privacy Limitations**
   - CDR shows only metadata, not conversation content
   - Cannot hear what was discussed
   
2. **Location Accuracy**
   - Cell tower location is approximate (500m - 2km)
   - Not precise GPS coordinates
   - Accuracy depends on tower density
   
3. **Call Forwarding Issues**
   - Can't differentiate direct calls from forwarded calls
   
4. **VoIP Calls**
   - May not appear in CDR (depends on provider)
   
5. **International Roaming**
   - Limited data for roaming scenarios
   
6. **Data Gaps**
   - CDR only covers periods of network activity
   - Does not show actual user location between calls

---

### Technical Questions

**Q8: How do you validate data consistency?**

A: Three-level validation:

1. **Input Validation**
   - Check file format (CSV)
   - Check file size (not corrupt)
   - Check headers exist
   
2. **Record Validation**
   - Phone numbers: Match +91-9xxx format
   - Dates: Between reasonable ranges
   - Duration: Positive numbers only
   - IMEI: 15 digits
   - IMSI: 15 digits
   
3. **Cross-Validation**
   - Total records in summary sheets = records in analysis
   - Contact counts match unique numbers found
   - Date ranges consistent

---

**Q9: How do you generate the Excel report?**

A: Using OpenPyXL:

```python
# Create workbook
wb = Workbook()

# Add summary sheet
ws_summary = wb.active
ws_summary.title = "Summary"
add_summary_data(ws_summary)

# Add analysis sheets (1 per module)
for module_name, dataframe in analysis_results:
    ws = wb.create_sheet(module_name)
    add_dataframe_to_sheet(ws, dataframe)
    apply_formatting(ws)

# Add styles
apply_header_format(wb)
apply_table_format(wb)

# Save
wb.save(report_path)
```

**Features**:
- Multiple sheets (summary + 20 analysis)
- Professional formatting (headers, colors)
- Data validation (drop-downs where applicable)
- Frozen header rows
- Auto-width columns

---

**Q10: How do you handle errors and failures?**

A: Three-level error handling:

1. **Graceful Degradation**
   - If one operator format fails, try next
   - If API is down, use fallback method
   - If optional field missing, continue with warning
   
2. **Detailed Logging**
   ```python
   logger.info(f"Processing {filename}")
   logger.warning(f"Encoding detected: {encoding}")
   logger.error(f"Failed to parse {count} records")
   logger.critical(f"Database connection failed")
   ```
   
3. **User Feedback**
   - Progress bar shows real-time progress
   - Detailed logs show what happened
   - Report shows summary of issues
   - Clear error messages

---

### Interview Red Flags to Avoid

**DON'T say**:
- ❌ "I just used default pandas without optimization"
- ❌ "I hardcoded the operator detection"
- ❌ "The tool only works with perfect CSV files"
- ❌ "I don't handle errors"

**DO say**:
- ✅ "I use vectorized operations for performance"
- ✅ "I auto-detect the operator format"
- ✅ "I handle various data quality issues"
- ✅ "Every operation is logged and recoverable"

---

## Interview Story Template

**Tell this story when asked about a challenging project:**

> "I built a forensic analysis tool for law enforcement because I saw they were spending days manually analyzing telecom data. 
>
> The biggest challenge was that each telecom provider (Airtel, Jio, Vi, BSNL) uses completely different formats - different column names, date formats, encoding. 
>
> So I built an automatic operator detector that analyzes the file structure and applies the correct parsing rules. It's actually kind of like training a simple classifier - try each pattern, see which one works best.
>
> Then I handle all the messy real-world data - missing values, encoding issues, invalid records - and clean it automatically.
>
> Finally, I analyze the data from 20+ different angles: contacts, devices, locations, time patterns. Special feature - Day/Night Halt analysis - can pinpoint where someone works (daytime) and lives (nighttime) just from tower data.
>
> The whole process that used to take 20+ hours now takes about 10 minutes. The tool is actively used by law enforcement agencies."

---

## Key Points to Remember

1. **Problem-Driven**: Built because there's a real need
2. **Automated**: Removes manual work
3. **Accurate**: Handles data quality issues intelligently
4. **Comprehensive**: 20+ investigation angles
5. **Professional**: Excel reports ready for court
6. **Scalable**: Handles large datasets efficiently
7. **Domain Knowledge**: Understands forensic requirements

---

## Numbers to Mention in Interview

- **4 operators** supported (automatic detection)
- **20+ analysis sheets** generated
- **99%+ data accuracy** after cleaning
- **50,000+ records** handled in ~10 minutes
- **156+ contacts** analyzed per investigation
- **5-section reports** (Summary, Raw, Parsed, Verified, Limitations)
- **3+ hours** saved per investigation

Good luck! 🎯

