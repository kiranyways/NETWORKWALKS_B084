# Security Assessment Report - Mediroza General Hospital

**Target:** https://medirozahospital.com

**Date:** 27 November 2026

**Tester:** Kiran Nawaz

**Batch:** B084 - NETWORKWALKS Cybersecurity Internship

**Scope:** Black-box Assessment - Simulated Target

**Status:** Findings Only - No Fixes Applied

---

## 1. Executive Summary

A black-box security assessment was performed on `medirozahospital.com` as part of the Week 4 capstone project.

A total of **9 findings** were identified, including **3 CRITICAL** issues related to exposed sensitive directories, exposed database backup, and SQL Injection.

The website uses a custom CMS `Mediroza CMS 1.4.2` on a `LiteSpeed` server. No fixes were applied as per instructions.

The most severe impact was achieved during **M3 - Sensitive Data Exposure**, where an old database backup file was publicly downloaded from the `/old/` directory, exposing confidential staff salaries and shareholder records.

---

## 2. Methodology & Tools Used

**M1 - Initial Access / Reconnaissance:**
- `whois` - Domain Registration Info
- `dnsrecon` - DNS Enumeration
- `nslookup` - DNS Lookup
- `curl` - Header & Response Analysis
- `wafw00f` - WAF Detection
- `whatweb` - Technology Fingerprinting
- `nikto` - Vulnerability Scanning
- `Zenmap / Nmap` - Network Scanning
- Manual Review - `robots.txt` & `sitemap.xml`

**M1 - Authentication Bypass:**
- SQL Injection Testing - Patient Portal Login

**M2 - Cracking / Post-Exploitation:**
- PDF Hash Extraction & Password Cracking - Patient Reports
- PDF Decrypted Content Analysis

**M3 - Sensitive Data Exposure:**
- Directory Enumeration - Gobuster - `/old/` Directory Discovery
- Database Backup Analysis - `.sql` File Review
- Staff Salaries Extraction
- Shareholder Details Extraction

All tests were performed on the provided simulated target for learning purposes only.

---

## 3. Findings Summary

| # | Finding | Severity | Status |
| --- | --- | --- | --- |
| 1 | Directory Indexing Enabled on /patient/, /staff/, /old/ | CRITICAL | Open |
| 2 | Sensitive Database Backup Exposed in /old/ - M3 | CRITICAL | Open |
| 3 | Sensitive Directories Disclosed in robots.txt | HIGH | Open |
| 4 | Missing Security Headers | MEDIUM | Open |
| 5 | Server & CMS Version Disclosure | LOW | Open |
| 6 | BREACH Attack Possible (HTTP Compression) | LOW | Open |
| 7 | Email Address Disclosure | INFO | Open |
| 8 | SQL Injection in Patient Portal Login | CRITICAL | Open |
| 9 | Sensitive PDFs Exposed via Directory Listing | HIGH | Open |

---

## 4. Detailed Findings

### [CRITICAL] 1. Directory Indexing Enabled

**Affected URLs:**
- `https://medirozahospital.com/patient/`
- `https://medirozahospital.com/staff/`
- `https://medirozahospital.com/old/`

**Evidence (Nikto):**
[750500] /patient/: Directory indexing found.
[750500] /staff/: Directory indexing found.
[750500] /old/: Directory indexing found.

**Risk:** Anyone can view the file listing inside hospital patient, staff, and old directories. This can lead to sensitive data exposure.

**Recommendation:** Disable directory indexing on the server. Review to ensure no sensitive files are publicly accessible.

### [CRITICAL] 2. Sensitive Database Backup Exposed in /old/ - M3 Sensitive Data Exposure

**Affected URL:**
- `https://medirozahospital.com/old/mediroza_db_backup_2019.sql`

**Evidence:**
The file was publicly accessible without any authentication and was downloaded directly from the directory listing. The file header contains the following warning:
> `WARNING: contains confidential staff and shareholder records`

**Content Found:**
- `staff` table with columns: `full_name, job_title, department, email, phone, national_id, monthly_salary_zar, date_joined`
- `shareholder` table with confidential shareholder records
- Sensitive personal data including National IDs, phone numbers, email addresses, and monthly salaries

**Risk:** This is a complete data breach. An attacker can download the full database backup and steal staff personal information, commit identity theft, and use salary and contact details for targeted phishing and social engineering. Exposure of shareholder data also creates financial and legal risk.

**Recommendation:** Immediately remove all backup files from public web directories. Store backups in a secure, offline location with restricted access. Audit access logs to check if this file was downloaded by unauthorized users.

### [HIGH] 3. Sensitive Directories Disclosed in robots.txt

**Evidence:**
Disallow: /patient/
Disallow: /staff/
Disallow: /old/

**Risk:** `robots.txt` publicly reveals the location of sensitive areas, helping attackers to target them.

**Recommendation:** Remove sensitive paths from robots.txt and protect them with authentication at server level.

### [MEDIUM] 4. Missing Security Headers

**Missing Headers:**
- `X-Frame-Options`
- `Content-Security-Policy`
- `Strict-Transport-Security`
- `X-Content-Type-Options`
- `Referrer-Policy`
- `Permissions-Policy`

**Risk:** Website is vulnerable to Clickjacking, XSS, and MIME-sniffing attacks.

**Recommendation:** Implement recommended security headers on LiteSpeed server.

### [LOW] 5. Server & CMS Version Disclosure

**Evidence:**
- `Server: LiteSpeed`
- `X-Turbo-Charged-By: LiteSpeed`
- `Mediroza CMS 1.4.2`
- `IP: 199.188.201.16`

**Risk:** Attacker can easily search for known vulnerabilities for this specific version.

**Recommendation:** Hide server version banners and CMS version meta tags.

### [LOW] 6. BREACH Attack Possible

**Evidence (Nikto):**
> Server may be vulnerable to BREACH attack due to `Content-Encoding: deflate`

**Risk:** Compressed responses may allow attacker to extract secrets like CSRF tokens.

**Recommendation:** Review disabling HTTP compression for sensitive pages or implement CSRF mitigations.

### [INFO] 7. Email Disclosure

**Evidence:** `info@medirozahospital.com` found via WhatWeb

**Risk:** Can be used for phishing / spam.

### [CRITICAL] 8. SQL Injection in Patient Portal Login - M1 Authentication Bypass

**Affected URL:** Patient Portal Login Page

**Payload Tested:** `admin' --` as username, any value as password

**Risk:** Attacker can bypass authentication without valid credentials and access patient portal. This leads to unauthorized access to patient data.

**Recommendation:** Use parameterized queries / prepared statements. Implement input validation and WAF rules for SQL injection patterns.

### [HIGH] 9. Sensitive PDFs Exposed via Directory Listing - M2 Cracking

**Evidence:**
- `patient_report_1.pdf`
- `patient_report_2.pdf`
- `patient_report_3.pdf`

Downloaded from `/patient/` directory listing.

**PDF Cracking Analysis:**
All 3 PDFs were password-protected. Hashes were extracted and cracked successfully, proving weak passwords were used for sensitive medical reports.

- PDF 1 - Password cracked: `123456`
- PDF 2 - Password cracked: `password`
- PDF 3 - Password cracked: Symbols-based weak password

After cracking, decrypted patient content was exposed, confirming patient privacy violation.

**Risk:** Patient reports are publicly downloadable. Exposure of medical reports violates patient privacy.

**Recommendation:** Remove public access to patient reports. Protect PDFs with strong passwords and proper access control.

---

## 5. Evidence - M1 Initial Access

**Whois Findings:**
- Domain: MEDIROZAHOSPITAL.COM
- Registrar: NameCheap, Inc.
- Created: 2026-08-14
- Name Server: DNS1.NAMECHEAPHOSTING.COM

**Dnsrecon Findings:**
- IP: 199.188.201.16
- Name Server: dns1.namecheaphosting.com
- Mail Server: mx3-hosting.jellyfish.systems
- 12 SRV Records found

---

## 6. Positive Findings

- SSL/TLS is correctly configured - `TLS_AES_256_GCM_SHA384` (Strong)
- Website returns `200 OK` for legitimate browser requests
- WAF / Bot Protection is active - Blocks default scanner User-Agents with `403 Forbidden`

---

## 7. Conclusion

The main priority is to fix **Finding #1 (Directory Indexing)**, **Finding #2 (Database Backup Exposure - M3)**, and **Finding #8 (SQL Injection)** immediately, as they expose hospital internal directories, allow login bypass, and expose a full confidential database backup.

The discovery of `mediroza_db_backup_2019.sql` in the `/old/` directory proves the real-world impact of directory indexing. What started as a simple directory listing led to complete exposure of staff salaries and shareholder data.

The remaining findings should be reviewed by the Mediroza IT Department.

*No fixes were applied during this assessment as instructed.*

---

## 8. Intern Reflection - Week 4

Just wrapped up Week 4 of my cybersecurity internship with NETWORKWALKS (Batch B084), and my mind is officially blown!

This week's capstone was a black-box penetration test on a simulated healthcare target: Mediroza General Hospital. The mission was clear: think like an attacker, find the vulnerabilities, and document everything professionally.

Here is what this intense week taught me:
- **The smallest details matter most:** The biggest breakthrough didn't come from a fancy automated tool. It came from digging into headers, robots.txt, and metadata.
- **Adaptability is a hacker's best friend:** When standard scanning methods were blocked by WAF with 403 Forbidden, I had to pivot and adjust my strategy. One size does not fit all.
- **Reporting is half the job:** Finding a vulnerability is only step one. Learning how to translate technical flaws into clear business risks and actionable remediation steps is a crucial skill.
- **Post-Exploitation shows real impact:** Finding a directory is one thing, but downloading a database backup proves why that vulnerability is truly critical.
