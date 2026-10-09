Project Overview

This project presents a Web Application Security Review and Vulner
ability Analysis performed in an authorized and isolated cybersecurity laboratory environment.

The assessment focuses on identifying common web application and web server security weaknesses, mapping findings to the OWASP Top 10, evaluating risk, and providing practical security recommendations.

The project was performed using Kali Linux, DVWA, Metasploitable 2, VMware Workstation, Nmap, Burp Suite, Nikto, and Gobuster.

 Disclaimer: This project was conducted only against an intentionally vulnerable laboratory environment owned/authorized for security testing. No unauthorized systems or public websites were tested.

 Project Objectives

The main objectives of this project were:

Understand the architecture and behavior of a web application.
Perform reconnaissance and service enumeration.
Review authentication and session management.
Identify common web application security weaknesses.
Perform controlled testing for SQL Injection and Cross-Site Scripting.
Enumerate web directories and server resources.
Identify web server misconfigurations and outdated components.
Map security findings to the OWASP Top 10.
Assess the likelihood and business impact of identified risks.
Provide practical security recommendations.
Prepare a professional web security audit report.
 Lab Environment
Component	Details
Operating System	Kali Linux
Target System	Metasploitable 2
Web Application	Damn Vulnerable Web Application (DVWA)
Virtualization	VMware Workstation
Network	Isolated Host-Only Network
Target IP	192.168.23.129
Web Server	Apache HTTP Server
Application Server	Apache Tomcat
Testing Type	Authorized Security Assessment
🛠️ Tools Used
Nmap – Host discovery and service/version enumeration
Burp Suite – HTTP traffic analysis and session review
Nikto – Web server security enumeration
Gobuster – Directory and resource enumeration
DVWA – Controlled web application security testing
Kali Linux – Security testing platform
VMware Workstation – Isolated laboratory environment
🔍 Testing Methodology

The assessment followed a structured security testing methodology:

Reconnaissance
      ↓
Service Enumeration
      ↓
Application Mapping
      ↓
Authentication Review
      ↓
Session Management Review
      ↓
Web Server Enumeration
      ↓
Directory Enumeration
      ↓
Input Validation Testing
      ↓
OWASP Risk Mapping
      ↓
Risk Assessment
      ↓
Security Recommendations
      ↓
Final Audit Report
🔎 Assessment Activities
1. Host Discovery

Nmap was used to identify active systems within the authorized host-only network.

nmap -sn 192.168.23.0/24

The authorized Metasploitable 2 target was identified at:

192.168.23.129

Evidence:

01_host_discovery_nmap.png

2. Service and Version Enumeration

The following command was used to identify services running on the target:

nmap -sV 192.168.23.129

Relevant web services identified included:

HTTP on port 80
Apache HTTP Server
HTTP service on port 8180
Apache Tomcat

Evidence:

02_service_version_scan.png

3. DVWA Application Assessment

The Damn Vulnerable Web Application (DVWA) was used as the controlled application for web security testing.

The application modules reviewed included:

Brute Force
Command Execution
CSRF
File Inclusion
SQL Injection
SQL Injection (Blind)
File Upload
Reflected XSS
Stored XSS
DVWA Security

The DVWA security level was configured to Low for controlled laboratory testing.

4. Burp Suite HTTP Traffic Analysis

Burp Suite was configured as an intercepting proxy between the browser and the authorized DVWA application.

HTTP history was reviewed to understand:

HTTP methods
Request URLs
Parameters
Headers
Cookies
Session-related information
Application request/response behavior

Sensitive session values were excluded from the documented evidence.

Evidence:

06_burp_http_history.png

5. Authentication Assessment

Controlled authentication tests were performed using valid and invalid login attempts.

Observations
A valid authentication attempt was successfully accepted.
An intentionally incorrect password resulted in authentication failure.
Authentication responses were reviewed for application behavior and error handling.

No automated password attacks were performed.

Evidence:

07_authentication_valid.png
08_authentication_invalid.png
6. Session Management Review

Burp Suite was used to inspect session-related HTTP cookies during authenticated application activity.

The review focused on security attributes such as:

Secure
HttpOnly
SameSite
Session identifier handling

Actual session values were not included in the public repository.

Evidence:

09_session_management_burp.png

🛡️ Web Server Security Assessment
7. Nikto Web Server Enumeration

Nikto was used to identify web server configuration and security issues.

Command:

nikto -h http://192.168.23.129
Key observations
Outdated Apache HTTP Server version
Outdated PHP version
Directory indexing
Accessible phpinfo.php
Exposed phpMyAdmin resource
HTTP TRACE functionality enabled
Missing recommended HTTP security headers
Additional development/test resources

These observations indicate weaknesses in server hardening and information disclosure.

Evidence:

10_nikto_web_enumeration.png

8. Gobuster Directory Enumeration

Gobuster was used to identify commonly accessible directories and resources.

Command:

gobuster dir -u http://192.168.23.129 -w /usr/share/wordlists/dirb/common.txt
Identified resources

Examples included:

phpinfo.php
phpMyAdmin
test
twiki
dav
index.php

Several sensitive-looking resources returned HTTP 403 Forbidden, indicating that access was denied by the web server.

Evidence:

11_gobuster_directory_enumeration.png

🧪 Vulnerability Testing
9. Reflected XSS Assessment

A controlled reflected XSS test was performed against the DVWA application.

The supplied JavaScript test input was not executed by the browser and no XSS popup was observed.

Result

Reflected XSS: Not Confirmed

The functionality was documented for further security review rather than reported as a confirmed vulnerability.

Evidence:

12_reflected_xss_test.png

10. Stored XSS Assessment

A controlled stored XSS test was performed against the DVWA application.

The test input did not execute JavaScript in the browser and no popup was observed.

Result

Stored XSS: Not Confirmed

Evidence:

13_stored_xss_test.png

11. SQL Injection Assessment

A baseline request using User ID 1 successfully returned the corresponding user record.

A controlled SQL injection test was then performed.

Result

The response did not show expanded query results or other evidence confirming successful SQL injection.

SQL Injection: Not Confirmed

Evidence:

14_sql_injection_test.png

📊 Key Security Findings
Finding	Evidence	OWASP Category	Risk
Outdated Apache/PHP	Nikto	A06 – Vulnerable and Outdated Components	High
Exposed phpMyAdmin	Nikto/Gobuster	A05 – Security Misconfiguration	High
Directory Listing	Nikto/Gobuster	A05 – Security Misconfiguration	Medium
Exposed phpinfo.php	Nikto/Gobuster	A05 – Security Misconfiguration	Medium
HTTP TRACE Enabled	Nikto	A05 – Security Misconfiguration	Medium
Missing Security Headers	Nikto	A05 – Security Misconfiguration	Medium
Authentication Review	DVWA	A07 – Identification and Authentication Failures	Informational
Session Management Review	Burp Suite	A07 – Identification and Authentication Failures	Informational
Reflected XSS	DVWA	A03 – Injection	Not Confirmed
Stored XSS	DVWA	A03 – Injection	Not Confirmed
SQL Injection	DVWA	A03 – Injection	Not Confirmed
⚠️ Risk Assessment
Finding	Likelihood	Impact	Overall Risk	Priority
Outdated Apache/PHP	High	High	High	P1
Exposed phpMyAdmin	Medium	High	High	P1
Directory Listing	Medium	Medium	Medium	P2
Exposed phpinfo.php	Medium	Medium	Medium	P2
HTTP TRACE Enabled	Medium	Medium	Medium	P2
Missing Security Headers	Medium	Medium	Medium	P2
Authentication Review	Low	Medium	Low/Informational	P3
Session Management Review	Low	Medium	Low/Informational	P3
🔐 Security Recommendations
1. Upgrade Outdated Components

Upgrade unsupported Apache and PHP versions to actively supported versions and maintain regular patch management.

2. Restrict phpMyAdmin

Administrative interfaces such as phpMyAdmin should not be publicly accessible. Access should be restricted using network controls, authentication, and appropriate administrative policies.

3. Disable Directory Listing

Directory indexing should be disabled to prevent unnecessary disclosure of application files and directory structures.

4. Remove phpinfo.php

Development and diagnostic files such as phpinfo.php should be removed from production systems.

5. Disable HTTP TRACE

HTTP TRACE should be disabled when it is not required.

6. Implement Security Headers

Appropriate security headers should be implemented, including:

Content-Security-Policy
X-Content-Type-Options
Referrer-Policy
Permissions-Policy
Strict-Transport-Security where HTTPS is enforced
7. Strengthen Authentication

Applications should implement strong password policies, account lockout/rate limiting, secure authentication workflows, and appropriate multi-factor authentication where applicable.

8. Protect Session Management

Session cookies should use appropriate security attributes such as:

Secure
HttpOnly
SameSite

Session identifiers should also be rotated appropriately after authentication.

9. Use Secure Input Handling

Applications should validate user input and use context-aware output encoding to reduce injection and XSS risks.

10. Use Parameterized Queries

Database operations should use parameterized queries or prepared statements rather than directly concatenating user-controlled input into SQL statements.

11. Regular Security Testing

Organizations should perform regular vulnerability assessments, penetration testing, dependency reviews, and configuration audits.

12. Remove Unnecessary Resources

Unused test applications, development files, documentation directories, and unnecessary services should be removed or restricted.

📁 Project Structure
web-application-security-audit/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── report/
│   ├── Web_Application_Security_Audit.pdf
│   └── Web_Application_Security_Audit.docx
│
├── screenshots/
│   ├── 01_host_discovery_nmap.png
│   ├── 02_service_version_scan.png
│   ├── 03_dvwa_login.png
│   ├── 04_dvwa_security_level_low.png
│   ├── 05_dvwa_application_mapping.png
│   ├── 06_burp_http_history.png
│   ├── 07_authentication_valid.png
│   ├── 08_authentication_invalid.png
│   ├── 09_session_management_burp.png
│   ├── 10_nikto_web_enumeration.png
│   ├── 11_gobuster_directory_enumeration.png
│   ├── 12_reflected_xss_test.png
│   ├── 13_stored_xss_test.png
│   └── 14_sql_injection_test.png
│
├── documentation/
│   ├── methodology.md
│   ├── scope.md
│   ├── testing-results.md
│   ├── vulnerability-findings.md
│   └── conclusion.md
│
├── owasp/
│   ├── owasp-top10-mapping.md
│   └── owasp-risk-mapping.xlsx
│
├── risk-assessment/
│   ├── risk-assessment.md
│   └── risk-matrix.png
│
├── recommendations/
│   └── security-recommendations.md
│
└── tools/
    └── tools-used.md
📄 Project Deliverables

The project includes:

Web Application Security Audit Report
OWASP Top 10 Risk Mapping
Risk Assessment
Security Recommendation Checklist
Testing Evidence Screenshots
Reconnaissance Results
Web Server Enumeration Results
Directory Enumeration Results
Authentication and Session Review
XSS and SQL Injection Testing Results
📚 OWASP Mapping

The assessment findings were mapped primarily to the following OWASP Top 10 categories:

A03 – Injection
A05 – Security Misconfiguration
A06 – Vulnerable and Outdated Components
A07 – Identification and Authentication Failures

The complete mapping is available in:

owasp/owasp-top10-mapping.md
📸 Evidence

Screenshots documenting the assessment process are available in:

screenshots/

The evidence includes:

Nmap host discovery
Nmap service enumeration
DVWA application
Burp Suite HTTP history
Authentication testing
Session review
Nikto enumeration
Gobuster enumeration
XSS testing
SQL Injection testing
📑 Full Report

The complete professional security assessment report is available here:

report/Web_Application_Security_Audit.pdf
🎓 Skills Demonstrated

This project demonstrates practical knowledge of:

Web Application Security
OWASP Top 10
Vulnerability Assessment
Security Auditing
Reconnaissance
Network Enumeration
Web Server Enumeration
Directory Enumeration
Authentication Testing
Session Management Review
SQL Injection Testing
XSS Testing
Risk Assessment
Security Hardening
Technical Documentation
Penetration Testing Methodology
⚖️ Ethical Use & Disclaimer

This repository is intended for educational and portfolio purposes.

All testing documented in this project was performed within an authorized, isolated laboratory environment using intentionally vulnerable systems.

Do not use the techniques or tools demonstrated in this repository against systems, applications, networks, or websites without explicit authorization.

Unauthorized security testing may be illegal.

👩‍💻 Author

Lakshmi Subhaja Kari

B.Tech – Cybersecurity

Aspiring Cybersecurity Engineer | Web Application Security | VAPT | Digital Forensics


