# Week 2 -- Footprinting and Network Scanning

## Selected Modules

-   **W2-PM1:** Footprinting with Multiple Kali Tools
-   **W2-PM5:** Zenmap-Based Network Scanning
-   **W2-PM-FINAL:** Detailed Report Covering the Selected Modules

**Student Name:** KIRAN NAWAZ
**Course / Section:** B084


## 1. Introduction

Footprinting and network scanning are important activities in cybersecurity. Footprinting collects publicly available information about a domain, while network scanning helps identify active hosts, open ports and running services.

In this project, Kali Linux tools were used to perform footprinting, and Zenmap was used for network scanning.

## 2. Objectives

- Understand the concept of footprinting.
- Use six Kali Linux tools to collect domain information.
- Identify website technologies and DNS records.
- Understand HTTP response headers.
- Learn how to detect a Web Application Firewall.
- Perform network scanning using Zenmap.
- Identify open ports and running services.

## 3. Environment and Tools

**Operating System:** Kali Linux

**Tools Used:**
- WHOIS
- WhatWeb
- NSLookup
- cURL
- WAFW00F
- DNSRecon
- Zenmap

**Scope and authorization:** Perform only the checks permitted by the
assignment and the target owner. The Zenmap scan in this report should
be performed against an authorized lab machine or another explicitly
approved target. Do not scan unrelated systems.

## 4. W2-PM1: Footprinting with Multiple Kali Tools

**Target Domain:** networkwalks.com

### 4.1 WHOIS

**Command:**
`whois networkwalks.com`

**Purpose:** To collect publicly available domain registration information, including registrar, registration dates and name servers.

### 4.2 WhatWeb

**Command:**
`whatweb https://networkwalks.com`

**Purpose:** To identify website technologies, web servers and content management systems.

### 4.3 NSLookup

**Commands:**
`nslookup networkwalks.com`

`nslookup -type=NS networkwalks.com`

**Purpose:** To find IP addresses and DNS name servers associated with the domain.

### 4.4 cURL

**Command:**
`curl -I https://networkwalks.com`

**Purpose:** To examine HTTP response headers and server responses.

### 4.5 WAFW00F

**Command:**
`wafw00f https://networkwalks.com`

**Purpose:** To check whether a Web Application Firewall is detected.

### 4.6 DNSRecon

**Command:**
`dnsrecon -d networkwalks.com -t std`

**Purpose:** To collect standard DNS records, including A, AAAA, MX, NS and TXT records where available.

## 5. PM1 Findings and Analysis

The six tools provide different types of information:

  Tool       Information collected
  ---------- --------------------------------------------
  WHOIS      Public domain registration information
  WhatWeb    Detectable website technologies
  NSLookup   DNS resolution and name-server information
  cURL       HTTP response status and headers
  WAFW00F    Whether a WAF can be detected
  DNSRecon   Published DNS records

## 6. W2-PM5: Zenmap Based Network Scanning

### 6.1 Introduction

Zenmap is the graphical user interface for Nmap. It makes it easier to
configure scans and view their results. Nmap can identify reachable
hosts, port states, and services. When service-version detection is
enabled, it also attempts to identify the versions of detected services.

### 6.2 Target Information

  Item                 Details
  -------------------- ------------------------------------
  Target name          \[Enter lab machine name\]
  Target IP address    \[Enter verified IP address\]
  Target environment   \[For example: Windows XP lab VM\]
  Scanning machine     Kali Linux
  Tool                 Zenmap
  Scan type            Service/version detection

### 6.3 Scan Command

``` bash
nmap -sV -T3 <authorized-target-ip>
```

Replace `<authorized-target-ip>` with the verified IP address of the
authorized lab machine.

**Command explanation**

-   `nmap`: starts the Nmap scanner.
-   `-sV`: attempts to identify services and their versions.
-   `-T3`: selects Nmap's normal timing template.
-   Target IP: specifies the machine being scanned.

### 6.4 Scan Procedure

1.  Open Zenmap in Kali Linux.
2.  Enter the authorized target IP address in the Target field.
3.  Select an appropriate scan profile.
4.  Check that the command targets the correct lab machine.
5.  Click **Scan**.
6.  Wait for the scan to complete.
7.  Review the **Nmap Output** and **Ports/Hosts** tabs.
8.  Record the actual port states and service information.

## 7. Security Observations

The practical activities demonstrate that publicly available domain
information and network scan results can help build an understanding of
a system's exposed information and services.

The results should be interpreted carefully:

-   Public registration and DNS information can reveal technical details
    about a domain.
-   Technology identification is based on indicators visible to the tool
    and may not reveal the complete technology stack.
-   HTTP headers can provide useful information about the server's
    responses and security configuration.
-   WAF detection tools may return an inconclusive result.
-   An open port indicates that a service is accepting connections; it
    does not automatically mean that the service is vulnerable.
-   Only authorized systems should be scanned.


## 8. Conclusion

In this project, I used six Kali Linux tools to perform footprinting and
collect publicly available information about a domain. I also used
Zenmap to perform network scanning on an authorized lab machine and
examine the detected ports and services.

The practical helped me understand the difference between footprinting
and network scanning, how to use common reconnaissance tools, and how to
interpret their results responsibly.

## 9. References

- Kali Linux Documentation: https://www.kali.org/docs/
- Nmap Documentation: https://nmap.org/book/man.html
- WHOIS: https://www.icann.org/resources/pages/whois-2013-07-17-en
- WhatWeb: https://github.com/urbanadventurer/WhatWeb
- DNSRecon: https://github.com/darkoperator/dnsrecon
- WAFW00F: https://github.com/EnableSecurity/wafw00f
