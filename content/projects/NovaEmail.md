---
title: 'NovaEmail - Secure Email Platform'
date: 2026-06-05
draft: false
tags: ["forensics", "metadata", "python"]
---


> **A self-hosted, full-featured email platform with React frontend, Express backend, custom SMTP server, and real-time notifications**

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [The Problem It Solves](#the-problem-it-solves)
3. [Key Features](#key-features)
4. [Architecture](#architecture)
5. [Technology Stack](#technology-stack)
6. [System Components](#system-components)
7. [Email Flow](#email-flow)
8. [Deployment](#deployment)
9. [Security Implementation](#security-implementation)
10. [Interview Q&A](#interview-qa)

---

## Project Overview

**NovaEmail** is a complete, self-hosted email platform you can deploy on your own server. Instead of relying on Gmail or Outlook, you host a full-featured email system with your own domain.

It's like building your own **Gmail alternative** with:
- Modern, responsive React web interface
- Express.js RESTful API backend
- Custom Node.js SMTP server for receiving emails
- MongoDB database for scalability
- SendGrid integration for reliable external email delivery
- Real-time WebSocket notifications
- Full email functionality (send, receive, search, organize)

### Quick Stats
- **Architecture**: Full-Stack (Frontend + Backend + SMTP + Infrastructure)
- **Language**: JavaScript (Node.js)
- **Frontend**: React 18 + TailwindCSS
- **Backend**: Express.js
- **Database**: MongoDB
- **Infrastructure**: Nginx + PM2 + Let's Encrypt SSL
- **Hosting**: Cloud (Azure Ubuntu Server)
- **Domain**: novaemail.me

---

## The Problem It Solves

### Why Build Your Own Email Platform?

**Problem 1: Privacy Concerns**
```
❌ Gmail/Outlook:
├─ Your emails stored on their servers
├─ They can scan content for ads
├─ Data breaches affect millions
└─ Government can request data

✅ NovaEmail:
├─ Your emails on YOUR server
├─ You control access completely
├─ Only you have encryption keys
└─ Complete privacy over your data
```

**Problem 2: No Control**
```
❌ Gmail:
├─ Can't modify features
├─ Can't control API
├─ Limited to their UI
└─ Dependent on their ToS

✅ NovaEmail:
├─ Full source code control
├─ Build custom features
├─ Modify UI exactly as needed
└─ Your rules, not theirs
```

**Problem 3: Cost**
```
❌ Enterprise Email Solutions:
├─ $20-30 per user per month
├─ 100 users = $2,400/month
└─ Additional features = additional cost

✅ NovaEmail:
├─ Single server = $5-20/month
├─ 1,000 users possible
├─ All features included
└─ One-time setup cost
```

**Problem 4: Lock-in**
```
❌ Switching from Gmail is painful:
├─ Exporting all emails takes hours
├─ Account recovery complex
├─ 3rd party tools required
└─ Data portability issues

✅ NovaEmail:
├─ Full data access (MongoDB)
├─ Standard formats
├─ Easy backup/restore
└─ Complete data ownership
```

### Real-World Use Cases

1. **Organizations Needing Privacy**
   - Law firms (client confidentiality)
   - Healthcare (HIPAA compliance)
   - Financial services (data security)

2. **Startups with Custom Needs**
   - Integration with existing systems
   - Custom workflows
   - Branded experience

3. **Government/Military**
   - Air-gapped deployments
   - Complete control over infrastructure
   - No 3rd party dependencies

---

## Key Features

### Email Functionality

#### **Sending Emails**
```
User composes in browser
      ↓
Attaches file (up to 25MB)
      ↓
Adds multiple recipients (chips UI)
      ↓
Sends via SendGrid SMTP
      ↓
External email delivered
```

- ✉️ Send to internal users (same domain)
- ✉️ Send to external addresses (Gmail, Outlook, etc.)
- 👥 Multiple recipients with Gmail-style chips
- 📎 Attachments (up to 25MB)
- 📝 Auto-save drafts
- ✅ Delivery confirmation

#### **Receiving Emails**
```
External sender (Gmail, etc.)
      ↓
Sends to your server (port 25)
      ↓
Custom SMTP server receives
      ↓
Parses email + attachments
      ↓
Scans with ClamAV (optional)
      ↓
Stores in MongoDB
      ↓
WebSocket notifies browser
      ↓
User sees notification
```

- 📧 Receive from any external email
- 🛡️ Virus scanning on attachments
- 📦 Automatic attachment storage
- ⏱️ Real-time notifications
- ✅ Delivery verification

#### **Email Organization**
```
Inbox (primary)
├─ Unread messages
├─ Auto-organized by date
└─ Real-time updates

Sent (outgoing)
├─ All emails sent by user
└─ Includes drafts that were sent

Drafts (in progress)
├─ Unsent messages
├─ Auto-saved every 30 seconds
└─ Auto-delete if empty for 7 days

Archive (kept but out of way)
├─ Intentionally archived emails
├─ Searchable
└─ Not deleted (recoverable)

Trash (deleted)
├─ 30-day auto-delete
├─ Can be recovered
└─ Free up space
```

#### **Email Search**
```
Search across:
- Subject line
- Sender/Recipient
- Email body content
- Attachment filenames

Example: "invoice from john 2024"
Result: All emails matching criteria
```

#### **Email Management**
- 🌟 Star important emails
- ✅ Mark as read/unread
- 📂 Move between folders
- 🔄 Bulk operations (select all, delete multiple)
- 🗑️ Soft delete (recoverable)
- 🔒 Secure deletion option

### User Interface Features

#### **Modern Design**
- 🎨 Dark theme (easy on eyes, professional)
- 📱 Mobile responsive (works on all devices)
- ⚡ Snappy performance (React optimization)
- 🎯 Intuitive (Gmail-like familiar UX)

#### **User Experience**
- 🖱️ Right-click context menu
  - Mark as read/unread
  - Star/unstar
  - Archive
  - Delete
- ✅ Bulk selection (checkboxes)
- 🔄 Keyboard shortcuts
- 🏃 Smooth animations

#### **Real-Time Features**
- 📬 Notification when new email arrives
- 🔔 Unread count badge
- ⚡ Live update without refresh
- 💬 WebSocket connection stays open

### Security Features

#### **Authentication**
```
Registration:
├─ Username (unique)
├─ Password (min 8 chars, bcrypt hash)
├─ Email (verified)
├─ Profile (name, DOB)
└─ Account created

Login:
├─ Username + Password
├─ Rate-limited (50 attempts per 15 min)
├─ JWT token issued
├─ Token stored in browser (secure cookie)
└─ Token used for API calls
```

#### **Password Security**
- 🔒 bcrypt hashing (not reversible)
- 🔐 Salt rounds: 10 (2^10 iterations)
- 🚫 No password storage in logs
- 🔄 Password reset via email
- 📋 Password strength indicator on registration

#### **Communication Security**
- 🔐 HTTPS (SSL certificate, auto-renewed)
- 🔒 Email encryption with SPF/DKIM/DMARC
- 🛡️ Helmet.js security headers
  - XSS Protection
  - CSRF Protection
  - Clickjacking Protection
- 🔐 JWT tokens with expiration

#### **Data Security**
- 🗄️ MongoDB password protected
- 🔑 API keys stored securely (environment variables)
- ✅ Input validation on all endpoints
- 🚫 SQL injection prevention
- 🛡️ Command injection prevention

#### **Email Authentication**
```
SPF (Sender Policy Framework):
├─ Tells receivers which servers can send email for your domain
├─ Prevents spoofing
└─ Record: v=spf1 ip4:YOUR_IP mx ~all

DKIM (DomainKeys Identified Mail):
├─ Digitally signs emails
├─ Receiver verifies signature
├─ Records from SendGrid configuration
└─ Proves email wasn't tampered

DMARC (Domain-based Message Authentication):
├─ Policy for how to handle failures
├─ Collects reporting from receivers
├─ Record: v=DMARC1; p=none; rua=mailto:admin@novaemail.me
└─ Monitor spoofing attempts
```

#### **Optional Virus Scanning**
```
ClamAV Integration:
├─ Scans attachment before storage
├─ Detects viruses/malware
├─ Quarantine infected files
└─ Alert administrator
```

---

## Architecture

### High-Level System Design

```
┌─────────────────────────────────────────────────────────┐
│                  User's Browser                         │
│           (React 18 + TailwindCSS Frontend)            │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ Compose  │ Inbox  │ Archive  │ Trash │ Settings│   │
│  │────────────────────────────────────────────────│   │
│  │ Email List      │ Email Content                │   │
│  │ Real-time       │ Attachment View              │   │
│  │ Updates         │ Reply Interface              │   │
│  └─────────────────────────────────────────────────┘   │
└────────────────────┬──────────────────────────────────┘
                     │ HTTPS + WebSocket
        ┌────────────▼─────────────┐
        │   Nginx Reverse Proxy    │
        │  (Port 80/443, Load Bal) │
        └────────────┬─────────────┘
                     │
    ┌────────────────┼────────────────┐
    │                │                │
    │        ┌───────▼────────┐       │        ┌──────────────┐
    │        │ Express API    │       │        │ SMTP Server  │
    │        │ (Port 5000)    │       │        │ (Port 25)    │
    │        │                │       │        │              │
    │        │ Routes:        │       │        │ Listens for  │
    │        │ /api/send      │       │        │ incoming     │
    │        │ /api/emails    │       │        │ emails       │
    │        │ /api/auth      │       │        │              │
    │        │ /api/register  │       │        │ MailParser   │
    │        │ /api/login     │       │        │ ClamAV       │
    │        │ /api/upload    │       │        │ MongoDB      │
    │        └───────┬────────┘       │        └──────┬───────┘
    │                │                │               │
    └────────────────┼────────────────┼───────────────┘
                     │                │
              ┌──────▼────────────────▼──────┐
              │     MongoDB Database         │
              │  (Collections)               │
              │  ├─ users                    │
              │  ├─ emails                   │
              │  ├─ attachments              │
              │  └─ sessions                 │
              └──────────────────────────────┘
                     │
                     │
        ┌────────────▼───────────┐
        │  SendGrid API          │
        │  (External email)      │
        │  (For outgoing emails) │
        └────────────────────────┘
```

### Component Breakdown

#### **Frontend (React)**
```
App.js (Main Component)
├── Auth Module
│   ├── Register Component
│   ├── Login Component
│   └── Profile Component
│
├── Email Module
│   ├── Inbox View
│   ├── Email Reader
│   ├── Compose Window
│   └── Email List
│
├── Folder Navigation
│   ├── Inbox
│   ├── Sent
│   ├── Drafts
│   ├── Archive
│   └── Trash
│
├── Features
│   ├── Search
│   ├── Star/Unstar
│   ├── Mark Read/Unread
│   ├── Move to Folder
│   └── Delete
│
└── Real-time Updates (WebSocket)
    └── New email notifications
```

#### **Backend (Express)**
```
server.js (Main Server)
├── Authentication
│   ├── POST /api/register (create account)
│   ├── POST /api/login (get JWT token)
│   └── Middleware: verify JWT
│
├── Email Operations
│   ├── GET /api/emails (list emails)
│   ├── GET /api/emails/:id (read single)
│   ├── POST /api/send (send new)
│   ├── POST /api/drafts (save draft)
│   ├── PUT /api/emails/:id/read (mark read)
│   ├── PUT /api/emails/:id/star (star email)
│   ├── PUT /api/emails/:id/move (move folder)
│   ├── DELETE /api/emails/:id (delete)
│   └── GET /api/stats (statistics)
│
├── File Upload
│   ├── POST /api/upload (attach file)
│   ├── Storage: /uploads/attachments/
│   └── Max size: 25MB
│
├── WebSocket
│   └── Socket.io for real-time notifications
│
├── Rate Limiting
│   ├── API: 100 req/15 min
│   └── Login: 50 attempts/15 min
│
└── Health & Monitoring
    └── GET /api/health (server status)
```

#### **SMTP Server (Custom Node.js)**
```
smtp-server.js (Independent Process)
├── Listens on Port 25
│
├── Incoming Email Flow
│   ├── Receives SMTP connection
│   ├── Accepts email for your domain
│   ├── MailParser extracts:
│   │   ├─ From (sender)
│   │   ├─ To (recipient)
│   │   ├─ Subject
│   │   ├─ Body
│   │   └─ Attachments
│   │
│   ├── Optional: ClamAV scan
│   │   ├─ Check for viruses
│   │   └─ Quarantine if infected
│   │
│   ├── Store in MongoDB
│   │   ├─ Create email record
│   │   ├─ Store metadata
│   │   └─ Link to recipient
│   │
│   └── Notify via WebSocket
│       └─ User sees notification
│
└── Process Management: PM2
    └─ Auto-restart on crash
```

---

## Technology Stack

### Frontend

```javascript
// React 18 - UI Framework
import React from 'react'

// TailwindCSS - Utility-first styling
<div className="flex gap-4 p-6 dark:bg-gray-900">

// Lucide React - Icon library
import { Mail, Send, Trash } from 'lucide-react'

// Fetch API - HTTP requests (no axios dependency)
const response = await fetch('/api/emails')

// WebSocket - Real-time updates
const socket = io(SERVER_URL)
socket.on('new-email', handleNewEmail)
```

### Backend

```javascript
// Node.js + Express - Server framework
const express = require('express')
const app = express()

// MongoDB + Mongoose - Database & ODM
const mongoose = require('mongoose')
const emailSchema = new Schema({
  from: String,
  to: [String],
  subject: String,
  body: String,
  attachments: [String],
  sentAt: Date
})

// JWT - Authentication tokens
const jwt = require('jsonwebtoken')
const token = jwt.sign({ userId }, JWT_SECRET, { expiresIn: '7d' })

// bcrypt - Password hashing
const hashedPassword = await bcrypt.hash(password, 10)

// Multer - File uploads
const upload = multer({ storage, limits: { fileSize: 25MB } })

// Nodemailer - Email sending
const transporter = nodemailer.createTransport(sendgridConfig)
await transporter.sendMail({ from, to, subject, html })

// Socket.io - Real-time WebSocket
const io = require('socket.io')(server)
io.on('connection', (socket) => { /* ... */ })

// Helmet - Security headers
const helmet = require('helmet')
app.use(helmet())

// Rate limiting
const rateLimit = require('express-rate-limit')
const limiter = rateLimit({ windowMs: 15 * 60 * 1000, max: 100 })
```

### SMTP Server

```javascript
// Node Mailer SMTP Server - Listen for incoming
const SMTPServer = require('smtp-server').SMTPServer
const server = new SMTPServer({
  disableReverseLookup: true,
  onData: async (stream, session) => {
    // Parse incoming email
  }
})
server.listen(25)

// MailParser - Parse email structure
const simpleParser = require('mailparser').simpleParser
const parsed = await simpleParser(stream)
```

### Infrastructure

```bash
# Nginx - Reverse proxy
upstream api_backend {
  server 127.0.0.1:5000;
}
server {
  listen 443 ssl http2;
  server_name novaemail.me;
  
  location / {
    proxy_pass http://api_backend;
  }
}

# PM2 - Process manager
pm2 start server.js --name novaemail-backend
pm2 start smtp-server.js --name novaemail-smtp
pm2 save

# Let's Encrypt - SSL certificates
certbot --nginx -d novaemail.me -d www.novaemail.me

# Firewall
ufw allow 22 (SSH)
ufw allow 80 (HTTP)
ufw allow 443 (HTTPS)
ufw allow 25 (SMTP)
```

---

## System Components

### 1. Frontend (React Application)

**Main Components**:
- **AuthPage**: Login/Register forms with validation
- **MainApp**: Email interface after login
- **EmailList**: Inbox with filters and search
- **EmailReader**: Single email view with reply
- **Composer**: Compose new email with attachments
- **FolderNav**: Switch between Inbox, Sent, Drafts, Archive, Trash

**Features**:
- Real-time email list updates
- Drag-and-drop file upload
- Keyboard shortcuts
- Auto-save drafts every 30 seconds
- Search across emails
- Responsive mobile view

**Performance Optimizations**:
- React lazy loading for components
- Virtual scrolling for large email lists
- Debounced search
- Cached API responses

### 2. Backend (Express API)

**Authentication Layer**:
```javascript
// Middleware verifies JWT on each request
router.use((req, res, next) => {
  const token = req.headers.authorization
  try {
    req.user = jwt.verify(token, JWT_SECRET)
    next()
  } catch (err) {
    res.status(401).json({ error: 'Invalid token' })
  }
})

// Routes require authentication
router.get('/api/emails', authenticate, getEmails)
```

**Core Routes**:
- `/api/auth/*` - User authentication
- `/api/emails/*` - Email operations
- `/api/upload` - File attachment upload
- `/api/stats` - User statistics
- `/api/health` - Server status

**Database Models** (Mongoose):
```javascript
// User schema
userSchema = {
  username: String (unique),
  passwordHash: String,
  email: String,
  profile: {
    firstName: String,
    lastName: String,
    dateOfBirth: Date
  }
}

// Email schema
emailSchema = {
  userId: ObjectId (indexed),
  from: String,
  to: [String],
  subject: String,
  body: String (HTML),
  attachments: [String],
  folder: String (inbox/sent/drafts/archive/trash),
  isRead: Boolean,
  isStarred: Boolean,
  sentAt: Date (indexed),
  createdAt: Date
}

// Attachment schema
attachmentSchema = {
  emailId: ObjectId,
  fileName: String,
  mimeType: String,
  size: Number,
  storagePath: String,
  uploadedAt: Date
}
```

**Indexes for Performance**:
```javascript
// Compound index: userId + folder + date
emailSchema.index({ userId: 1, folder: 1, sentAt: -1 })

// Unique email addresses per user
userSchema.index({ email: 1 }, { unique: true })

// Fast lookups by userId
emailSchema.index({ userId: 1 })
```

### 3. SMTP Server (Custom Implementation)

**How It Works**:
```
SMTP Connection from external sender
     ↓
Validate domain matches our server
     ↓
Accept MAIL FROM command
     ↓
Accept RCPT TO (if recipient exists)
     ↓
Receive DATA (email content)
     ↓
Parse with MailParser
     ├─ Extract From
     ├─ Extract To
     ├─ Extract Subject
     ├─ Extract Body (HTML + Text)
     └─ Extract Attachments
     ↓
Optional: ClamAV virus scan
     ├─ If infected → Quarantine
     └─ If clean → Continue
     ↓
Store in MongoDB
     ├─ Create email document
     ├─ Save attachments
     ├─ Set folder: inbox
     └─ Set isRead: false
     ↓
Notify via WebSocket
     ├─ Connect to Socket.io
     ├─ Find user by recipient
     └─ Send notification
     ↓
Return SMTP 250 OK (accepted)
```

**Port 25 Configuration**:
- Listens on all interfaces (0.0.0.0)
- Accepts connections for your domain
- Rejects external relaying (security)
- Processes emails concurrently

---

## Email Flow

### Sending Email (User → External)

```
Step 1: User composes in browser
│
├─ Fill fields:
│  ├─ To: (can add multiple)
│  ├─ Subject:
│  ├─ Body: (HTML editor)
│  └─ Attach: (files)
│
└─ Frontend validates
   ├─ Check To field not empty
   ├─ Check at least 1 recipient
   └─ Check Subject not empty

Step 2: Submit to backend
│
├─ POST /api/send
│  ├─ Headers: Authorization: Bearer JWT_TOKEN
│  ├─ Body: {
│  │   to: ['user@gmail.com'],
│  │   subject: 'Hello',
│  │   body: '<p>Email body</p>',
│  │   attachments: ['id1', 'id2']
│  │ }
│  └─ Verify JWT token
│
└─ Backend processes
   ├─ Validate recipient emails
   ├─ Fetch attachments from storage
   ├─ Prepare email object
   └─ Send via SendGrid

Step 3: SendGrid SMTP relay
│
├─ SendGrid receives email
├─ Validates domain reputation
├─ Adds DKIM signature
├─ Adds SPF headers
└─ Sends to recipient's mail server

Step 4: Recipient receives
│
├─ Email arrives at Gmail/Outlook
├─ Their server verifies:
│  ├─ SPF check (passes)
│  ├─ DKIM signature (passes)
│  └─ DMARC policy (passes)
└─ Email delivered to inbox

Step 5: Store in MongoDB
│
├─ Create email record:
│  ├─ from: novaemail.me
│  ├─ to: ['user@gmail.com']
│  ├─ status: 'sent'
│  ├─ folder: 'sent'
│  ├─ sentAt: timestamp
│  └─ messageId: unique_id
│
└─ User sees in Sent folder

Step 6: Return confirmation
│
└─ Frontend shows "Email sent"
```

### Receiving Email (External → User)

```
Step 1: External sender sends
│
├─ User at Gmail composes
├─ Recipient: test@novaemail.me
├─ Sends to Gmail SMTP
└─ Gmail forwards to novaemail.me

Step 2: Gmail SMTP Server
│
├─ DNS lookup: novaemail.me
├─ Gets MX record: mail.novaemail.me
├─ Connects to our server:25
└─ Sends SMTP DATA

Step 3: Our SMTP Server receives
│
├─ SMTPServer listens on port 25
├─ Accepts MAIL FROM: sender@gmail.com
├─ Accepts RCPT TO: test@novaemail.me
├─ Accepts DATA: email content
└─ Parses with MailParser:
│  ├─ from: sender@gmail.com
│  ├─ to: test@novaemail.me
│  ├─ subject: 'Hello'
│  ├─ body: '<p>Email content</p>'
│  └─ attachments: [...]
│
└─ Returns 250 OK (accepted)

Step 4: Optional virus scan
│
├─ ClamAV scans attachments
│  ├─ If clean → Continue
│  └─ If infected → Quarantine & alert
│
└─ Process continues

Step 5: Store in MongoDB
│
├─ Create email document:
│  ├─ from: sender@gmail.com
│  ├─ to: test@novaemail.me
│  ├─ subject: 'Hello'
│  ├─ body: HTML content
│  ├─ attachments: [file_refs]
│  ├─ folder: 'inbox'
│  ├─ isRead: false
│  ├─ isStarred: false
│  ├─ receivedAt: timestamp
│  └─ messageId: unique_id
│
└─ Document linked to user by email

Step 6: Real-time notification
│
├─ Connect to Socket.io
├─ Find logged-in user sessions
├─ Emit 'new-email' event:
│  ├─ from: sender@gmail.com
│  ├─ subject: 'Hello'
│  └─ preview: 'Email content...'
│
└─ Frontend shows notification

Step 7: User sees in browser
│
├─ Browser receives WebSocket message
├─ Notification pops up
├─ Inbox list refreshes
└─ New email appears at top
```

---

## Deployment

### Prerequisites
```
✓ Ubuntu 20.04 or later
✓ Node.js 18+
✓ MongoDB Atlas account (free tier)
✓ SendGrid account (100 emails/day free)
✓ Domain name (registered)
✓ Server ports open (22, 25, 80, 443)
```

### Deployment Steps

**Step 1: Get server access**
```bash
ssh ubuntu@YOUR_SERVER_IP
```

**Step 2: Clone code**
```bash
git clone https://github.com/YOUR_REPO/novaemail
cd novaemail
```

**Step 3: Install dependencies**
```bash
# Backend
cd backend
npm install

# Frontend
cd ../frontend
npm install
npm run build  # Creates production build

# SMTP
cd ../smtp-server
npm install
```

**Step 4: Configure environment**
```bash
# Create .env file
cat > backend/.env << EOF
PORT=5000
NODE_ENV=production
DOMAIN=novaemail.me
JWT_SECRET=$(openssl rand -base64 32)
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/novaemail
FRONTEND_URL=https://novaemail.me
SENDGRID_API_KEY=SG.xxxxxxxxxxxxx
EOF
```

**Step 5: Start services with PM2**
```bash
# Backend API
pm2 start backend/server.js --name novaemail-backend

# SMTP Server
pm2 start smtp-server/smtp-server.js --name novaemail-smtp

# Save PM2 config
pm2 save
pm2 startup
```

**Step 6: Configure Nginx**
```bash
# Copy config
sudo cp nginx/novaemail.conf /etc/nginx/sites-available/

# Enable site
sudo ln -s /etc/nginx/sites-available/novaemail /etc/nginx/sites-enabled/

# Remove default
sudo rm /etc/nginx/sites-enabled/default

# Test & restart
sudo nginx -t
sudo systemctl restart nginx
```

**Step 7: SSL Certificate**
```bash
sudo apt-get install certbot python3-certbot-nginx
sudo certbot --nginx -d novaemail.me -d www.novaemail.me
```

**Step 8: Configure DNS**
```
A Record:        novaemail.me → YOUR_SERVER_IP
MX Record:       novaemail.me → mail.novaemail.me (priority 10)
SPF Record:      novaemail.me → v=spf1 ip4:YOUR_IP mx ~all
DMARC Record:    _dmarc.novaemail.me → v=DMARC1; p=none; rua=mailto:admin@novaemail.me
DKIM Records:    (from SendGrid configuration)
```

**Step 9: Firewall**
```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 25/tcp
sudo ufw enable
```

**Step 10: Verify**
```bash
# Check services running
pm2 status

# Check logs
pm2 logs novaemail-backend
pm2 logs novaemail-smtp

# Test HTTP endpoint
curl http://localhost:5000/api/health

# Test SMTP
telnet localhost 25
```

---

## Security Implementation

### 1. Password Security

**Registration**:
```javascript
// User submits password
const password = req.body.password

// Validate strength
if (password.length < 8) throw new Error('Too short')
if (!/[A-Z]/.test(password)) throw new Error('Need uppercase')
if (!/[0-9]/.test(password)) throw new Error('Need number')

// Hash password (not reversible)
const salt = await bcrypt.genSalt(10)  // 2^10 iterations
const hashedPassword = await bcrypt.hash(password, salt)

// Store hash (not password)
user.password = hashedPassword
await user.save()
```

**Login**:
```javascript
// User submits password
const password = req.body.password

// Compare with stored hash
const match = await bcrypt.compare(password, user.password)

// No way to reverse hash - must do actual comparison
if (!match) throw new Error('Invalid password')
```

### 2. Authentication (JWT Tokens)

**Generate Token**:
```javascript
// After successful login
const token = jwt.sign(
  { userId: user._id, email: user.email },
  JWT_SECRET,
  { expiresIn: '7d' }  // Token expires in 7 days
)

// Send to client
res.json({ token })
```

**Use Token**:
```javascript
// Client sends token in headers
Authorization: Bearer eyJhbGc...

// Server verifies
const decoded = jwt.verify(token, JWT_SECRET)
req.user = decoded  // Attach to request

// If invalid or expired → 401 Unauthorized
```

### 3. Rate Limiting

**API Rate Limiting**:
```javascript
const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,  // 15 minutes
  max: 100,                   // 100 requests per window
  message: 'Too many requests, try again later'
})

app.use('/api/', apiLimiter)
```

**Login Rate Limiting** (stricter):
```javascript
const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,  // 15 minutes
  max: 50,                    // Only 50 login attempts
  skipSuccessfulRequests: true  // Don't count successful logins
})

app.post('/api/login', loginLimiter, handleLogin)
```

### 4. Email Authentication

**SPF (Sender Policy Framework)**:
```
DNS Record:
v=spf1 ip4:YOUR_SERVER_IP mx ~all

When Gmail receives email from your domain:
├─ Checks SPF record
├─ Verifies sending IP in list
├─ If match → SPF passes
└─ If no match → Possible spoofing
```

**DKIM (DomainKeys Identified Mail)**:
```
Your server signs emails with private key
Recipient verifies signature with public key
├─ Signature can't be forged (cryptography)
├─ Email hasn't been modified
├─ Proves your server sent it
└─ Gmail marks as authenticated
```

**DMARC (Domain-based Message Authentication, Reporting & Conformance)**:
```
Policy: What to do if SPF/DKIM fail
├─ p=none: Monitor only (don't block)
├─ p=quarantine: Send to spam
└─ p=reject: Block email completely

Reporting:
├─ Receivers send reports about authentication
├─ You can see SPF/DKIM failures
└─ Helps identify issues
```

### 5. HTTPS/TLS

```
Client sends request to novaemail.me:443
     ↓
Server presents SSL certificate (from Let's Encrypt)
     ↓
Client verifies certificate:
├─ Signed by trusted CA
├─ Matches domain
└─ Not expired
     ↓
TLS handshake completes
     ↓
All communication encrypted with AES-256
     ↓
No one can intercept requests/responses
     (Even if they intercept packets, encrypted)
```

### 6. Input Validation

**Email Field Validation**:
```javascript
// Validate email format
const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
if (!emailRegex.test(email)) {
  throw new Error('Invalid email format')
}

// Prevent NoSQL injection
if (typeof email !== 'string') {
  throw new Error('Email must be string')
}

// Prevent XSS in email content
const sanitized = DOMPurify.sanitize(emailBody)
```

**File Upload Validation**:
```javascript
// Check file size
if (file.size > 25 * 1024 * 1024) {
  throw new Error('File too large (max 25MB)')
}

// Check file type
const allowedTypes = ['image/*', 'application/pdf', 'text/*']
if (!allowedTypes.includes(file.type)) {
  throw new Error('File type not allowed')
}

// Store with random name (prevent directory traversal)
const filename = crypto.randomBytes(16).toString('hex') + '.bin'
```

### 7. Optional: ClamAV Virus Scanning

```javascript
// When attachment uploaded
const { exec } = require('child_process')

// Scan file
exec(`clamscan ${filePath}`, (error, stdout) => {
  if (error) {
    // Virus detected
    logger.error('Virus detected in file')
    quarantineFile(filePath)
    notifyAdmin()
  } else {
    // File clean
    allowDownload(filePath)
  }
})
```

---

## Interview Q&A

### Basic Questions

**Q1: Why did you build this project?**

A: I wanted to learn full-stack web development by building something real. Email systems are complex (SMTP, authentication, real-time updates, file uploads), so it was a great learning opportunity. Plus, there are legitimate use cases for self-hosted email - privacy-conscious organizations, teams needing integration with legacy systems, cost savings for large deployments.

---

**Q2: What's the most complex part of this project?**

A: Building the SMTP server to receive emails. It's not just receiving data - you need to:
1. Implement the SMTP protocol correctly
2. Parse complex email formats (MIME multipart, attachments, HTML)
3. Handle different encodings
4. Store efficiently in MongoDB
5. Notify the browser in real-time with WebSocket
6. Integrate optional ClamAV scanning

Most developers use Sendmail or Postfix (C-based servers). Doing it in Node.js required careful attention to the RFC 5321 spec.

---

**Q3: How do you handle sending emails to external servers (like Gmail)?**

A: I use SendGrid's SMTP gateway:
1. Backend connects to SendGrid SMTP server
2. Provides SendGrid API key for authentication
3. SendGrid handles actual delivery to Gmail/Outlook/etc
4. SendGrid provides DKIM/SPF authentication
5. Handles bounces and complaints

This outsources the hard parts - mail reputation, deliverability, authentication.

---

**Q4: How do you ensure emails aren't lost?**

A: Three layers:
1. **Receipt Confirmation**: SMTP server confirms email received
2. **Database Storage**: Immediately saved to MongoDB (replicated)
3. **Duplicate Detection**: messageId prevents storing same email twice
4. **Backups**: MongoDB Atlas handles backups automatically

---

**Q5: How do you prevent spam/abuse?**

A: Multiple approaches:
1. **Rate Limiting**: Max 100 API requests/15 min per user
2. **Login Throttling**: Max 50 login attempts/15 min
3. **Email Authentication**: SPF/DKIM/DMARC prevent spoofing
4. **ClamAV**: Optional virus/malware scanning
5. **IP Reputation**: SendGrid monitors sender reputation
6. **Bounce Handling**: Remove invalid addresses

---

**Q6: How many users can this support?**

A: Depends on hardware:

```
Single Server (~5-20/month cost):
├─ Capacity: 5,000-10,000 users
├─ 100 MB/s bandwidth
├─ MongoDB Atlas free tier: 512MB → 5GB
└─ Nginx load balancer: 10,000+ concurrent

Scale to millions:
├─ Load balance frontend (multiple instances)
├─ Load balance backend (multiple API servers)
├─ Load balance SMTP (multiple SMTP servers)
├─ Scale MongoDB (sharding)
└─ Use CDN for static assets
```

The architecture supports scaling by adding more servers.

---

### Technical Questions

**Q7: How do you handle real-time email notifications?**

A: WebSocket (Socket.io):

```javascript
// Backend
io.on('connection', (socket) => {
  socket.on('join-room', (userId) => {
    socket.join(`user_${userId}`)
  })
})

// When email arrives via SMTP
io.to(`user_${recipientId}`).emit('new-email', {
  from: sender,
  subject: subject,
  preview: snippet
})

// Frontend
socket.on('new-email', (data) => {
  // Show notification
  showNotification(data)
  // Refresh email list
  fetchEmails()
})
```

Benefits over polling:
- **Polling**: Refresh every 30 seconds (30s latency, wasteful)
- **WebSocket**: Instant notification (< 100ms latency, efficient)

---

**Q8: How do you design the email schema?**

A: MongoDB schema with proper indexing:

```javascript
{
  _id: ObjectId,
  userId: ObjectId,           // Owner of email
  from: 'sender@gmail.com',
  to: ['recipient@novaemail.me'],
  cc: [],
  bcc: [],
  subject: 'Hello',
  body: '<p>Email HTML content</p>',
  textBody: 'Email plain text',  // For plain text clients
  
  // Metadata
  messageId: 'unique@sender',  // For deduplication
  folder: 'inbox',            // inbox|sent|drafts|archive|trash
  isRead: false,
  isStarred: false,
  
  // File references
  attachments: [
    {
      originalName: 'invoice.pdf',
      mimeType: 'application/pdf',
      size: 1024000,
      storagePath: '/uploads/abc123.bin',
      uploadedAt: Date
    }
  ],
  
  // Timestamps
  sentAt: Date,
  receivedAt: Date,
  createdAt: Date,
  updatedAt: Date
}

// Indexes
db.emails.createIndex({ userId: 1, folder: 1, sentAt: -1 })
db.emails.createIndex({ userId: 1, isRead: 1 })
db.emails.createIndex({ messageId: 1 }, { unique: true })
```

Index strategy:
- `(userId, folder, sentAt)` compound - Fast for inbox queries
- `(userId, isRead)` - Fast for unread count
- `messageId` unique - Prevent duplicates

---

**Q9: How do you handle file uploads securely?**

A: Multiple validations:

```javascript
const upload = multer({
  storage: diskStorage({
    destination: (req, file, cb) => {
      cb(null, '/uploads/attachments/')
    },
    filename: (req, file, cb) => {
      // Use random name, not original filename
      const name = crypto.randomBytes(16).toString('hex')
      cb(null, name + '.bin')
    }
  }),
  
  limits: {
    fileSize: 25 * 1024 * 1024  // 25MB max
  },
  
  fileFilter: (req, file, cb) => {
    // Allowed types
    const allowed = ['image/*', 'application/pdf', 'text/*']
    
    if (allowed.includes(file.mimetype)) {
      cb(null, true)
    } else {
      cb(new Error('File type not allowed'))
    }
  }
})

// Use middleware
router.post('/api/upload', authenticate, upload.single('file'))
```

Security measures:
- Store with random name (not user input)
- Validate file size
- Validate MIME type
- Store outside web root (not directly accessible)
- Scan with ClamAV (optional)

---

**Q10: How do you prevent XSS attacks in email content?**

A: 

```javascript
// Option 1: DOMPurify (recommended)
const DOMPurify = require('isomorphic-dompurify')
const cleanEmail = DOMPurify.sanitize(emailBody, {
  ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'p', 'br', 'a'],
  ALLOWED_ATTR: ['href', 'target'],
  KEEP_CONTENT: true
})

// Option 2: Escape HTML
const escape = (html) => {
  return html
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#039;')
}

// Frontend: Use innerHTML with caution
// UNSAFE: innerHTML = userInput
// SAFE: textContent = userInput (doesn't parse HTML)
// SAFE: dangerouslySetInnerHTML with sanitized content (React)
```

The goal: Allow safe HTML (links, formatting) but prevent `<script>` tags.

---

### Deployment Questions

**Q11: How do you handle SSL certificate renewal?**

A: Certbot auto-renewal:

```bash
# Initial certificate
sudo certbot --nginx -d novaemail.me

# Certbot sets up auto-renewal
sudo systemctl enable certbot.timer

# Manual renewal (if needed)
sudo certbot renew

# Dry run (test without making changes)
sudo certbot renew --dry-run

# Monitor renewal
sudo systemctl list-timers certbot.timer
```

Certbot automatically:
- Checks 30 days before expiration
- Renews if expiring soon
- Reloads Nginx
- No downtime required

---

**Q12: How do you monitor the running services?**

A: PM2 monitoring:

```bash
# View status
pm2 status

# View logs
pm2 logs novaemail-backend
pm2 logs novaemail-smtp

# Monitor in real-time
pm2 monit

# Save memory/CPU snapshots
pm2 save

# Get diagnostics
pm2 diagnose
```

Monitor key metrics:
- CPU usage (should be <20% idle)
- Memory (should not increase over time)
- Restart count (should be 0)
- Error logs (check for repeating errors)

---

### Problem-Solving Questions

**Q13: An email gets stuck in "Drafts" and can't be sent. How do you debug?**

A: Systematic troubleshooting:

```javascript
// 1. Check if email exists in database
db.emails.findOne({ _id: emailId })
// If not found → Database connection issue

// 2. Check email status
const email = await Email.findById(emailId)
console.log(email)  // Check all fields
// Missing required fields → Frontend validation

// 3. Try sending via API directly
POST /api/send
{
  to: ['test@gmail.com'],
  subject: 'Test',
  body: '<p>Test</p>'
}
// If fails → Backend/SendGrid issue

// 4. Check SendGrid API
curl https://api.sendgrid.com/v3/mail/send \
  -X POST \
  -H "Authorization: Bearer SENDGRID_KEY" \
  -H "Content-Type: application/json" \
  -d '{...}'
// Check response code

// 5. Review logs
pm2 logs novaemail-backend | grep error
// Check for exceptions

// 6. Check MongoDB connection
// If MongoDB down → Can't save/retrieve emails

// 7. Check SendGrid quota
// If over limit → Can't send more emails
```

---

**Q14: Why is email taking 30 seconds to appear after I send it?**

A: Multiple potential causes:

```
Sending takes 3 seconds
├─ API call overhead
├─ SendGrid SMTP handshake
└─ MongoDB write

Receiving takes 5-10 seconds
├─ External server's processing
├─ Network propagation
├─ DNS resolution (MX lookup)
└─ SMTP server queueing

Display takes 2-5 seconds
├─ Email list polling (30s interval)
├─ WebSocket broadcasting
├─ React re-render

Total: Up to 40+ seconds is normal

To optimize:
├─ Reduce polling interval (but more wasteful)
├─ Use WebSocket for real-time (already done)
├─ Use SendGrid webhooks for delivery confirmation
└─ Add optimistic updates in UI (show immediately)
```

---

## Interview Story

**Tell this when asked about your biggest project:**

> "I built NovaEmail - a complete self-hosted email platform. This taught me full-stack web development from database to deployment.
>
> **The Challenge**: Email systems are complicated. You need:
> - A web interface (React)
> - An API (Express)
> - A custom SMTP server to RECEIVE emails (most complex part)
> - Database design (MongoDB)
> - Real-time notifications (WebSocket)
> - Security (authentication, encryption, email validation)
> - Infrastructure (Nginx, SSL, firewall)
>
> **The Solution**: I built each component step by step:
> 1. Frontend: React UI with TailwindCSS
> 2. Backend: Express API with JWT auth
> 3. SMTP: Custom Node.js SMTP server (implementing RFC 5321)
> 4. Infrastructure: Nginx reverse proxy + SSL + PM2 process management
>
> **Key Technical Decisions**:
> - Used Socket.io for real-time notifications instead of polling (much more efficient)
> - Implemented compound indexes in MongoDB for fast queries
> - Outsourced outgoing email to SendGrid (focus on receiving)
> - Used bcrypt for password hashing (non-reversible)
> - Implemented proper rate limiting on login
>
> **Results**:
> - Complete working email platform
> - Supports multiple users
> - Real-time notifications
> - Secure authentication
> - All features of a production email system
>
> This project gave me deep understanding of how email actually works."

---

## Numbers to Remember

- **Attachment limit**: 25MB
- **Rate limit API**: 100 requests per 15 minutes
- **Rate limit login**: 50 attempts per 15 minutes
- **JWT expiration**: 7 days
- **bcrypt salt rounds**: 10 (2^10 iterations)
- **Default polling interval**: 30 seconds
- **Auto-save drafts**: Every 30 seconds
- **Database storage**: MongoDB Atlas (5GB free)
- **Email delivery**: SendGrid (100/day free)
- **Supported users**: 5,000-10,000 per single server

Good luck with your interview! 🚀
