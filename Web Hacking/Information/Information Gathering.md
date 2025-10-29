# Infomration Gathering
<img width="2058" height="747" alt="image" src="https://github.com/user-attachments/assets/00757660-6975-4472-99b9-96cece608b5c" />

# Types of Reconnaissance
## Active Reconnaissance
- Port Scanning: Using Nmap to scan a web server for open ports like 80 (HTTP) and 443 (HTTPS).
- Vulnerability Scanning: Running Nessus against a web application to check for SQL injection flaws or cross-site scripting (XSS) vulnerabilities.
- Network Mapping: Using traceroute to determine the path packets take to reach the target server, revealing potential network hops and infrastructure.
- Banner Grabbing: Connecting to a web server on port 80 and examining the HTTP banner to identify the web server software and version.
- OS Fingerprinting: Using Nmap's OS detection capabilities (-O) to determine if the target is running Windows, Linux, or another OS.
- Service Enumeration: Using Nmap's service version detection (-sV) to determine if a web server is running Apache 2.4.50 or Nginx 1.18.0.
- Web Spidering: Running a web crawler like Burp Suite Spider or OWASP ZAP Spider to map out the structure of a website and discover hidden resources.

## Passive Reconnaissance
- Search Engine Queries: Searching Google for "[Target Name] employees" to find employee information or social media profiles.
- WHOIS Lookups: Performing a WHOIS lookup on a target domain to find the registrant's name, contact information, and name servers.
- DNS: Using dig to enumerate subdomains of a target domain.
- Web Archive Analysis: Using the Wayback Machine to view past versions of a target website to see how it has changed over time.
- Social Media Analysis: Searching LinkedIn for employees of a target organisation to learn about their roles, responsibilities, and potential social engineering targets.
- Code Repositories: Searching GitHub for code snippets or repositories related to the target that might contain sensitive information or code vulnerabilities.

# WHOIS:
- Phishing Investigation
- Malware Analysis
- Threat Intelligence Report

# DNS
## DNS TOOLS
- dig: Versatile DNS lookup tool that supports various query types (A, MX, NS, TXT, etc.) and detailed output.
- nslookup: Simpler DNS lookup tool, primarily for A, AAAA, and MX records.
- host: Streamlined DNS lookup tool with concise output.
- dnsenum: Automated DNS enumeration tool, dictionary attacks, brute-forcing, zone transfers (if allowed).
- fierce: DNS reconnaissance and subdomain enumeration tool with recursive search and wildcard detection.
- dnsrecon: Combines multiple DNS reconnaissance techniques and supports various output formats.
- theHarvester: OSINT tool that gathers information from various sources, including DNS records (email addresses).
- Online DNS Lookup Services:	User-friendly interfaces for performing DNS lookups.	

## Subdomains vs Vhost:
- Subdomains: These are extensions of a main domain name (e.g., blog.example.com is a subdomain of example.com). Subdomains typically have their own DNS records, pointing to either the same IP address as the main domain or a different one. They can be used to organise different sections or services of a website.
- Virtual Hosts (VHosts): Virtual hosts are configurations within a web server that allow multiple websites or applications to be hosted on a single server. They can be associated with top-level domains (e.g., example.com) or subdomains (e.g., dev.example.com). Each virtual host can have its own separate configuration, enabling precise control over how requests are handled.

Example:

Subdomains:

<img width="914" height="420" alt="image" src="https://github.com/user-attachments/assets/6768f42b-a60b-46be-9eec-18254ba6a8c1" />

Zone Transfer

<img width="970" height="500" alt="image" src="https://github.com/user-attachments/assets/ac0e26e4-b34b-4e28-bc6a-910d097a1ef3" />

Vhost:

<img width="1264" height="478" alt="image" src="https://github.com/user-attachments/assets/7fc29f6d-e623-440d-90db-bb1c74bb209c" />

# Fingerprinting
Identify target system's infrastructre

## Conerstone:
- Target Attackts
- Identifying Missconfigurations
- Prioritising Targets
- Building a comprehensive profile

## Tools
- Wappalyzer
- BuiltWith: Provide detailed reports on a webtsite's technologies
- WhatWeb
- Nmap
- Netcraft: Provide website fingerprinting and security reporting
- wafw00f: Identifying WAFs
- nikto: Web server scanner

# Well-Known URIs
```json
{
  "issuer": "https://example.com",
  "authorization_endpoint": "https://example.com/oauth2/authorize",
  "token_endpoint": "https://example.com/oauth2/token",
  "userinfo_endpoint": "https://example.com/oauth2/userinfo",
  "jwks_uri": "https://example.com/oauth2/jwks",
  "response_types_supported": ["code", "token", "id_token"],
  "subject_types_supported": ["public"],
  "id_token_signing_alg_values_supported": ["RS256"],
  "scopes_supported": ["openid", "profile", "email"]
}
```
