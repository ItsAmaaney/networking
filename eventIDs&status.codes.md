# SOC Cheat Sheet - Important HTTP Status Codes & Windows Event IDs

This note contains the most important codes every SOC Analyst should know.

---

# HTTP Status Codes

## 2xx - Success

| Code | Meaning | SOC Relevance |
|------|---------|---------------|
| **200** | OK | Successful request. May indicate successful login or page access. |
| **201** | Created | Resource successfully created. |

---

## 3xx - Redirection

| Code | Meaning | SOC Relevance |
|------|---------|---------------|
| **301** | Moved Permanently | Permanent redirect. |
| **302** | Found (Temporary Redirect) | Temporary redirect after login or page movement. |
| **304** | Not Modified | Cached resource used. Usually not security-related. |

---

## 4xx - Client Errors

| Code | Meaning | SOC Relevance |
|------|---------|---------------|
| **400** | Bad Request | Malformed request. |
| **401** | Unauthorized | Authentication failed (wrong credentials). |
| **403** | Forbidden | Access denied after authentication or blocked by policy. |
| **404** | Not Found | Missing resource. High volumes may indicate directory enumeration or vulnerability scanning. |
| **405** | Method Not Allowed | Invalid HTTP method used. |
| **408** | Request Timeout | Client timed out. |
| **429** | Too Many Requests | Rate limiting; may indicate brute-force or automated attacks. |

---

## 5xx - Server Errors

| Code | Meaning | SOC Relevance |
|------|---------|---------------|
| **500** | Internal Server Error | Server-side failure. |
| **502** | Bad Gateway | Upstream server error. |
| **503** | Service Unavailable | Service unavailable; may occur during outages or DoS attacks. |
| **504** | Gateway Timeout | Backend server did not respond in time. |

---

# Windows Security Event IDs

## Authentication

| Event ID | Meaning | Importance |
|----------|---------|------------|
| **4624** | Successful Logon | ⭐⭐⭐⭐⭐ |
| **4625** | Failed Logon | ⭐⭐⭐⭐⭐ |
| **4634** | Logoff | ⭐⭐⭐ |
| **4647** | User Initiated Logoff | ⭐⭐ |
| **4648** | Logon Using Explicit Credentials | ⭐⭐⭐⭐ |
| **4672** | Special Privileges Assigned | ⭐⭐⭐⭐⭐ |

---

## Account Management

| Event ID | Meaning | Importance |
|----------|---------|------------|
| **4720** | User Account Created | ⭐⭐⭐⭐⭐ |
| **4722** | User Account Enabled | ⭐⭐⭐⭐ |
| **4723** | Password Change Attempt | ⭐⭐⭐⭐ |
| **4724** | Password Reset Attempt | ⭐⭐⭐⭐⭐ |
| **4725** | User Account Disabled | ⭐⭐⭐ |
| **4726** | User Account Deleted | ⭐⭐⭐⭐⭐ |
| **4732** | User Added to Security Group | ⭐⭐⭐⭐⭐ |
| **4733** | User Removed from Security Group | ⭐⭐⭐⭐ |

---

## Process Creation

| Event ID | Meaning | Importance |
|----------|---------|------------|
| **4688** | Process Created | ⭐⭐⭐⭐⭐ |
| **4689** | Process Ended | ⭐⭐⭐ |

---

## Object Access

| Event ID | Meaning | Importance |
|----------|---------|------------|
| **4656** | Handle to Object Requested | ⭐⭐⭐ |
| **4663** | Object Accessed | ⭐⭐⭐⭐ |

---

## Policy & Audit

| Event ID | Meaning | Importance |
|----------|---------|------------|
| **4719** | System Audit Policy Changed | ⭐⭐⭐⭐⭐ |
| **1102** | Audit Log Cleared | ⭐⭐⭐⭐⭐ |

---

## PowerShell

| Event ID | Meaning | Importance |
|----------|---------|------------|
| **4103** | PowerShell Module Logging | ⭐⭐⭐⭐ |
| **4104** | PowerShell Script Block Logging | ⭐⭐⭐⭐⭐ |

---

## Sysmon Event IDs (Very Important)

| Event ID | Meaning | Importance |
|----------|---------|------------|
| **1** | Process Creation | ⭐⭐⭐⭐⭐ |
| **3** | Network Connection | ⭐⭐⭐⭐⭐ |
| **7** | Image Loaded | ⭐⭐⭐ |
| **8** | Create Remote Thread | ⭐⭐⭐⭐ |
| **10** | Process Access | ⭐⭐⭐⭐ |
| **11** | File Created | ⭐⭐⭐⭐ |
| **13** | Registry Value Set | ⭐⭐⭐⭐ |
| **22** | DNS Query | ⭐⭐⭐⭐⭐ |

---

# Common SOC Investigations

## Brute Force

Look for:

- HTTP **401**
- HTTP **429**
- Windows **4625**
- Followed by **4624**

---

## Successful Login

Look for:

- HTTP **200** (application dependent)
- Windows **4624**

---

## Failed Login

Look for:

- HTTP **401**
- Windows **4625**

---

## Privilege Escalation

Look for:

- **4672**
- **4732**
- **4720**

---

## Suspicious Process Execution

Look for:

- **4688**
- Sysmon **1**

---

## PowerShell Abuse

Look for:

- **4103**
- **4104**

---

## Log Tampering

Look for:

- **1102**

---

## Web Enumeration / Vulnerability Scanning

Look for:

- HTTP **404**
- Many requests to:
  - `/admin`
  - `/phpmyadmin`
  - `/wp-login.php`
  - `/.env`
  - `/.git`
- High request rate from the same IP

---

# High-Priority Codes to Memorize

## HTTP

- **200** → Success
- **401** → Unauthorized
- **403** → Forbidden
- **404** → Not Found
- **429** → Too Many Requests
- **500** → Internal Server Error
- **503** → Service Unavailable

---

## Windows

- **4624** → Successful Logon
- **4625** → Failed Logon
- **4672** → Special Privileges
- **4688** → Process Created
- **4720** → User Created
- **4732** → User Added to Group
- **4719** → Audit Policy Changed
- **1102** → Audit Log Cleared
- **4104** → PowerShell Script Block Logging

---

# Key Takeaways

- **HTTP Status Codes** describe web server responses.
- **Windows Event IDs** describe Windows security events.
- **Sysmon Event IDs** provide detailed endpoint activity.
- Always identify the log source before interpreting a code.
- In SOC investigations, understand the context—not just the code.