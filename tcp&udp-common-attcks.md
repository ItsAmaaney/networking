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

## 1️⃣ Reconnaissance (Scanning)

### 📌 Definition
Recon is the process of discovering targets, endpoints, and vulnerabilities.

### 🎯 Goal
- Find weak points (admin panels, backups, configs)

### ⚙️ How It Works
- Automated tools scan endpoints and ports
- Try common paths (/admin, /backup.zip, etc.)

### 🔍 Log Indicators
- Many 404 / 403 responses
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
- Multiple failed logins
- Same username
- Short time interval
- Possible success at end 🚨

### 🌐 Protocols Used
- HTTP/HTTPS
- SSH (22), RDP (3389)

### 🚨 Detection Tips
- Monitor login failures
- Detect spikes in authentication attempts

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

## 🧠 Key Analyst Mindset

- Don’t rely on one indicator ❌  
- Correlate multiple signals ✅  
- Focus on patterns, not just definitions  
