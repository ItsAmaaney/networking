# Networking Basics for SOC

---

## DNS (Domain Name System)

### What is DNS?
DNS converts a domain name into an IP address.

Example:
google.com → 142.250.x.x

---

### How DNS Works
1. User enters a domain (google.com)
2. Request goes to DNS resolver
3. Resolver returns IP address
4. Browser connects to that IP.

---

### SOC Perspective

What to look for:
- High number of DNS requests
- Requests to random domains
- Repeated failed DNS queries

Possible threats:
- (mainly) DNS tunneling  [ Using DNS to secretly send data ]
- Malware beaconing  [ Infected system keeps contacting attacker again and again ]
- Data exfiltration [ Stealing data from your system ]

Example:
100 DNS queries in 5 seconds → Suspicious

---

## TCP 3-Way Handshake

### What is it?
Process used to establish a TCP connection.

---

### Steps:
1. SYN → Client initiates
2. SYN-ACK → Server responds
3. ACK → Client confirms

Connection established

---

### SOC Perspective

What to look for:
- Many SYN requests without ACK
- Half-open connections
- Unexpected RST packets

Possible threats:
- SYN Flood (DoS)
- Network scanning

Example:
Thousands of SYN requests without completion → SYN Flood

---

## IP Address Basics

### What is an IP?
An IP address is a logical address. it identifies a device in a network.

Example:
192.168.1.10

---

### Private IP Ranges
- 192.168.x.x
- 10.x.x.x
- 172.16 – 172.31.x.x

---

### Subnet Example
192.168.1.0/24

- Network Address → 192.168.1.0
- Usable Hosts → 192.168.1.1 – 192.168.1.254
- Broadcast Address → 192.168.1.255

---

### SOC Perspective

What to look for:
- One IP communicating with many hosts
- Traffic between subnets
- External IP accessing internal systems

Possible threats:
- Internal scanning
- Lateral movement
- Malware spread

Example:
192.168.1.5 contacting multiple hosts → Suspicious

---

## Key Takeaway

SOC is NOT about:
- Memorizing definitions

SOC is about:
- Detecting abnormal behavior
- Understanding traffic patterns
- Investigating incidents
