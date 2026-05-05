# 🌐 TCP vs UDP - SOC Notes

---

## 🔌 TCP (Transmission Control Protocol)

### 📌 Definition
TCP is a **connection-oriented and reliable protocol** used for secure and accurate data transmission.

### ⚙️ Key Features
- Uses **3-way handshake** (SYN → SYN-ACK → ACK)
- Guarantees **delivery of packets**
- Ensures **correct order of data**
- Retransmits lost packets
- Higher overhead → **slower but reliable**

### 🌐 Common Uses
- HTTP / HTTPS (Web traffic)
- SSH (Remote login)
- FTP (File transfer)
- SMTP (Email)

### 🧠 SOC Relevance
- Detect **SYN scans** (recon activity)
- Track **sessions easily**
- Identify **C2 beaconing over HTTP/HTTPS**
- Useful in **log correlation**

---

## ⚡ UDP (User Datagram Protocol)

### 📌 Definition
UDP is a **connectionless and fast protocol** with no guarantee of delivery.

### ⚙️ Key Features
- No handshake (no connection setup)
- No guarantee of delivery
- No packet ordering
- Low overhead → **very fast**

### 🌐 Common Uses
- DNS (Domain resolution)
- DHCP (IP assignment)
- VoIP (Calls)
- Streaming / Gaming

### 🧠 SOC Relevance
- Detect **DNS tunneling (C2 communication)**
- Used in **DDoS attacks (UDP flood)**
- Harder to analyze (no sessions)
- Requires **pattern-based detection**

---

## ⚔️ TCP vs UDP (Key Differences)

| Feature        | TCP                          | UDP                         |
|---------------|------------------------------|------------------------------|
| Connection    | Connection-oriented          | Connectionless               |
| Reliability   | Reliable (guaranteed)        | Unreliable                   |
| Ordering      | Maintains order              | No ordering                  |
| Speed         | Slower                       | Faster                       |
| Overhead      | High                         | Low                          |
| Detection     | Easier (stateful tracking)   | Harder (stateless)           |

---

## 🚨 SOC Detection Patterns

### 🔍 TCP Indicators
- Multiple **SYN packets, no ACK** → Port scanning
- Repeated connections → Brute force / automation
- Regular interval traffic → C2 beaconing

### 🔍 UDP Indicators
- High DNS traffic (port 53) → Possible tunneling
- Random subdomains → Data exfiltration
- Large UDP spikes → DDoS attack

---

## 🧪 Analyst Thinking (IMPORTANT)

Never analyze protocol alone. Always correlate:
- Source IP
- Destination IP/domain
- Port number
- Frequency
- Payload pattern

---

# 🔐 SOC Attack Notes (Core Concepts)

---

### 🔥 FINAL MUST-MEMORIZE
200 → exists  
401 → login needed  
403 → exists but blocked  
404 → not found  
500 → server error

### In SOC pov 📊 
404 → recon  
403 → interesting target  
200 → valid entry point  
401 → auth needed

# HTTP Methods (SOC Perspective)

## 🎯 Objective
Understand HTTP methods and how they appear in logs for security analysis.

---

## 🔹 GET
**Purpose:** Request data from server  

**SOC Insight:**
- Normal browsing activity  
- High volume → possible reconnaissance (endpoint scanning)

**Example:**
- Accessing web pages  
- Viewing resources  

---

## 🔹 POST
**Purpose:** Send data to server  

**SOC Insight:**
- Common in authentication (login attempts)  
- Multiple POST requests → possible brute force attack  

**Example:**
- Login requests  
- Form submissions  

---

## 🔹 PUT
**Purpose:** Replace or update data  

**SOC Insight:**
- Unexpected usage → suspicious  
- May indicate unauthorized modification  

---

## 🔹 DELETE
**Purpose:** Remove data  

**SOC Insight:**
- High risk action 🚨  
- Unexpected DELETE requests → possible malicious activity  

---

## 🔹 PATCH
**Purpose:** Partially update data  

**SOC Insight:**
- Used for modifying specific data  
- Monitor for unusual changes  

---

## 🔹 HEAD
**Purpose:** Retrieve headers only (no content)  

**SOC Insight:**
- Can be used to check resource existence  
- Repeated use → possible reconnaissance  

---

## 🔹 OPTIONS
**Purpose:** Check allowed methods  

**SOC Insight:**
- Used in normal browser/API behavior  
- Excessive requests → possible probing activity  

---

## 🔥 Key Takeaways

```
GET → Read (normal / recon if excessive)  
POST → Send (login attempts / brute force)  
PUT/PATCH → Modify (watch for unauthorized changes)  
DELETE → Remove (high risk 🚨)  
HEAD/OPTIONS → Probe (recon activity)
```

---

## 🧠 Analyst Thinking

```
Method + Pattern + Context = Detection
```

## 1️⃣ Reconnaissance (Scanning)

### 📌 Definition
Recon is the process of discovering targets, endpoints, and vulnerabilities.

### 🎯 Goal
- Find weak points (admin panels, backups, configs)

### ⚙️ How It Works
- Automated tools scan endpoints and ports
- Try common paths (/admin, /backup.zip, etc.)

### 🔍 Log Indicators
- Many 404 / 403 responses and mainly (GET) req can be viewed mostly in logs
- Multiple endpoints in short time
- No successful access
- Random/sensitive paths

### 🌐 Protocols Used
- HTTP/HTTPS
- TCP (port scanning)

### 🚨 Detection Tips
- Look for repeated failed requests
- Identify unusual endpoints
- Check request frequency
- 
  ## 🛠️ What to Check if detected
- Any 200 responses (successful access) 🚨
- Request frequency (automation?)
- User-agent (tool or browser?)
- Same IP continuing activity?

### 🧪 Example
GET /admin → 404            
GET /backup.zip → 404  
GET /phpmyadmin → 404  

---

## 2️⃣ Brute Force Attack

### 📌 Definition
Trying multiple passwords repeatedly to gain access.

### 🎯 Goal
- Break into accounts/systems

### ⚙️ How It Works
- Automated login attempts
- Trial and error

### 🔍 Log Indicators
- Multiple failed logins and mostly are shown as post or login req in logs
- Same username
- Short time interval
- Possible success at end 🚨

### 🌐 Protocols Used
- HTTP/HTTPS
- SSH (22), RDP (3389)

### 🚨 What analyst should watch next
- Continued login attempts from same IP  
- Any successful login after failures  
- Unusual login times or locations  
- Increase in authentication failures across users


### Attack responce
- Block malicious IP (after confirmation)
- Lock account / apply rate limiting
- Check IP reputation & geolocation
- Analyze user-agent (bot vs browser)
- Check if multiple accounts are targeted
- Force password reset (if risk)
- Enable MFA (Multi-Factor Authentication)
- Review post-login activity (if any)
- Monitor for continued attempts
- Confirm attack has stopped


### 🧪 Example
user=admin → failed  
user=admin → failed  
user=admin → success 🚨  

---

## 3️⃣ DoS / DDoS Attack

### 📌 Definition
Overwhelming a system to make it unavailable.

### 🎯 Goal
- Exhaust server resources (CPU, memory, bandwidth)

### ⚙️ How It Works
- Flooding requests to server
- DoS = single source
- DDoS = multiple sources

### 🔍 Log Indicators
- High traffic spike
- Repeated requests to same endpoint
- Slow server response
- Many IPs (DDoS)

### 🌐 Protocols Used
- TCP, UDP, HTTP

### 🚨 Detection Tips
- Compare traffic baseline
- Check IP distribution
- Monitor server performance

### 🧪 Example
50,000 requests/min → /login  
Server slow 🚨  

---

## 4️⃣ DNS Tunneling / C2

### 📌 Definition
Using DNS to hide communication between infected system and attacker.

### 🎯 Goal
- Data exfiltration
- Command & Control (C2)

### ⚙️ How It Works
- Malware encodes data into DNS queries
- Sends to attacker-controlled server

### 🔍 Log Indicators
- High DNS request frequency
- Random/encoded subdomains
- Repeated queries to same domain

### 🌐 Protocols Used
- UDP (port 53)

### 🚨 Detection Tips
- Monitor DNS traffic patterns
- Look for unusual domain names
- Check endpoint generating requests (EDR)

### 🧪 Example
x8s9d2k.data.attacker.com  
(repeated queries) 🚨  

---
## 🔐 Credential Stuffing

## 📌 Definition
Credential stuffing is an attack where an attacker uses **already stolen usernames and passwords** (from data breaches) to log into accounts.

- No guessing involved
- Uses **valid credentials**

---

## 🧠 Core Idea
- Brute Force = guessing passwords  
- Password Spraying = trying common passwords across many users  
- Credential Stuffing = using **real leaked credentials**

---

## 🔍 Log Pattern
- LOGIN attempts (mostly **SUCCESS** or very few failures)
- Often **no failed attempts before success**
- Multiple user accounts may be targeted
- IP behavior:
  - single IP OR
  - multiple/distributed IPs

---

## 🚨 Key Indicators
- Successful login **without brute-force pattern**
- Login from:
  - unusual IP
  - new location / country
- Multiple accounts accessed in similar way
- Activity looks legitimate but is abnormal for user

---

## 🧪 Post-Login Signals (CRITICAL)
After login, attacker may:
- Change password
- Add recovery email
- Disable MFA / security settings
- Access or download sensitive data

→ Indicates **Account Takeover (ATO)**

---

## 🔎 How to Confirm
- Check login history (new IP / geo anomaly)
- Compare with user's normal behavior
- Look for **no failure pattern before success**
- Correlate multiple accounts accessed
- Analyze post-login actions

---

## ⚠️ Detection Challenge
- No noisy failures (unlike brute force)
- Looks like normal login
- Requires **context-based analysis**

---

## 🎯 SOC Response
- Lock affected account
- Force password reset
- Revoke active sessions
- Investigate accessed/downloaded data
- Enable MFA
- Monitor for further suspicious activity

---

## 🧠 Key Takeaway
> "Success ≠ Safe"
A login can be successful and still be a compromise.

## 🧠 Key Analyst Mindset

- Don’t rely on one indicator ❌  
- Correlate multiple signals ✅  
- Focus on patterns, not just definitions  
