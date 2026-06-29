# Network Reconnaissance with Nmap - Metasploitable Lab

![Nmap](https://img.shields.io/badge/Nmap-7.98-blue)
![Kali Linux](https://img.shields.io/badge/Kali-Linux-557C94)
![VirtualBox](https://img.shields.io/badge/Oracle-VirtualBox-2F61B4)
![Metasploitable](https://img.shields.io/badge/Target-Metasploitable-red)
![License](https://img.shields.io/badge/License-Educational-green)

---

# Network Reconnaissance Lab

This project demonstrates a basic network reconnaissance exercise performed using **Nmap** against a **Metasploitable** virtual machine in a controlled laboratory environment.

The objective was to identify exposed services, understand their purpose, and research potential security risks associated with outdated software versions commonly found in intentionally vulnerable systems.

> **Disclaimer**
>
> This project was conducted exclusively inside an isolated virtual laboratory for educational purposes. All scans were performed against a deliberately vulnerable machine (**Metasploitable**) running in Oracle VirtualBox.

---

# Table of Contents

* Overview
* Lab Environment
* Objectives
* Tools Used
* Network Topology
* Scan Methodology
* Nmap Command
* Scan Results
* Service Analysis
* Security Findings
* Packet Capture
* Key Takeaways
* References
* Ethical Disclaimer

---

# Overview

Network reconnaissance is one of the first phases of a penetration test. Before attempting any exploitation, security professionals identify active hosts, discover open ports, determine running services, and fingerprint operating systems.

For this exercise:

* **Attacker:** Kali Linux VM
* **Target:** Metasploitable 2 VM
* **Virtualization:** Oracle VirtualBox
* **Technique:** TCP SYN Scan using Nmap

---

# Lab Environment

| Component        | Description         |
| ---------------- | ------------------- |
| Host OS          | Windows             |
| Hypervisor       | Oracle VirtualBox   |
| Attacker Machine | Kali Linux          |
| Target Machine   | Metasploitable 2    |
| Network          | Host-Only Adapter   |
| Scan Tool        | Nmap 7.98           |
| Packet Capture   | Wireshark (.pcapng) |

---

# Network Topology

```text
                Oracle VirtualBox

        +-----------------------------+
        |        Windows Host         |
        +-------------+---------------+
                      |
               Host-Only Network
                      |
        +-------------+-------------+
        |                           |
+-------------------+      +----------------------+
|   Kali Linux VM   | ---> |  Metasploitable VM   |
|   Nmap Scanner    |      | Vulnerable Target    |
+-------------------+      +----------------------+
```

---

# Objectives

* Discover active hosts
* Enumerate open TCP ports
* Identify running services
* Detect service versions
* Fingerprint the operating system
* Research known vulnerabilities
* Understand the security implications of exposed services

---

# Tools Used

* Nmap
* Wireshark
* Kali Linux
* Oracle VirtualBox
* Metasploitable 2

---

# Scan Methodology

The scan was executed from the Kali Linux virtual machine against the Metasploitable host using Nmap's service detection capabilities.

The scan identified:

* Open ports
* Service versions
* Operating system information
* Device fingerprint
* Network distance

---

# Nmap Command

```bash
nmap -sV -O 192.168.56.103
```

Where:

* **-sV** → Service Version Detection
* **-O** → Operating System Detection

---

# Scan Summary

| Property         | Value          |
| ---------------- | -------------- |
| Target IP        | 192.168.56.103 |
| Host Status      | Up             |
| Network Distance | 1 Hop          |
| Operating System | Linux 2.6.x    |
| Open TCP Ports   | 23             |
| Scan Time        | 14.30 seconds  |

---

# Open Ports

| Port | Service    | Version                   |
| ---- | ---------- | ------------------------- |
| 21   | FTP        | vsftpd 2.3.4              |
| 22   | SSH        | OpenSSH 4.7p1             |
| 23   | Telnet     | Linux telnetd             |
| 25   | SMTP       | Postfix                   |
| 53   | DNS        | ISC BIND 9.4.2            |
| 80   | HTTP       | Apache 2.2.8              |
| 111  | RPCBind    | v2                        |
| 139  | NetBIOS    | Samba                     |
| 445  | SMB        | Samba                     |
| 512  | rexec      | Netkit-rsh                |
| 513  | rlogin     | rlogind                   |
| 514  | rsh        | rshd                      |
| 1099 | Java RMI   | GNU Classpath             |
| 1524 | Bind Shell | Metasploitable Root Shell |
| 2049 | NFS        | RPC                       |
| 2121 | FTP        | ProFTPD 1.3.1             |
| 3306 | MySQL      | 5.0.51a                   |
| 5432 | PostgreSQL | 8.3.x                     |
| 5900 | VNC        | Protocol 3.3              |
| 6000 | X11        | X Server                  |
| 6667 | IRC        | UnrealIRCd                |
| 8009 | AJP13      | Apache JServ              |
| 8180 | HTTP       | Apache Tomcat             |

---

# Service Analysis

## Port 21 - FTP (vsftpd 2.3.4)

### Description

FTP is used for file transfer between systems.

### Security Risks

* Anonymous login
* Clear-text authentication
* File disclosure

### Known Vulnerabilities

* **CVE-2011-2523**
* Backdoored version of vsftpd 2.3.4
* Can provide unauthorized remote shell access.

---

## Port 22 - SSH (OpenSSH 4.7p1)

### Description

Encrypted remote administration service.

### Security Risks

* Weak passwords
* Brute-force attacks
* Legacy cryptographic algorithms

### Known Vulnerabilities

Older OpenSSH releases contain multiple historical vulnerabilities and should be upgraded.

---

## Port 23 - Telnet

### Description

Legacy remote administration protocol.

### Security Risks

* Credentials transmitted in plain text
* Easily intercepted using packet capture
* No encryption

---

## Port 25 - SMTP (Postfix)

### Description

Mail Transfer Agent responsible for email delivery.

### Security Risks

* User enumeration
* Open relay misconfiguration
* Spam abuse

---

## Port 53 - DNS (ISC BIND 9.4.2)

### Description

Domain Name System server.

### Security Risks

* Cache poisoning
* Zone transfer leakage
* Denial-of-Service vulnerabilities

---

## Port 80 - Apache HTTP Server 2.2.8

### Description

Web server hosting HTTP applications.

### Security Risks

* Directory traversal
* Outdated modules
* Remote information disclosure

### Notes

Apache 2.2.x reached end-of-life many years ago.

---

## Port 111 - RPCBind

### Description

Maps RPC services to network ports.

### Security Risks

* Service enumeration
* NFS discovery
* Information leakage

---

## Ports 139 & 445 - Samba

### Description

Windows-compatible file sharing service.

### Security Risks

* Anonymous shares
* SMB enumeration
* Credential attacks

### Known Vulnerabilities

**CVE-2007-2447**

The Samba "username map script" vulnerability allows remote command execution on vulnerable versions.

---

## Ports 512, 513 and 514 - R Services

### Description

Legacy remote execution protocols.

* rexec
* rlogin
* rsh

### Security Risks

* No encryption
* Trust-based authentication
* Credential interception

These protocols are considered obsolete.

---

## Port 1099 - Java RMI

### Description

Java Remote Method Invocation registry.

### Security Risks

* Remote object enumeration
* Java deserialization attacks
* Remote Code Execution

---

## Port 1524 - Metasploitable Bind Shell

### Description

An intentionally installed root shell.

### Security Risks

* Direct root access
* No authentication
* Complete system compromise

This service exists specifically for penetration-testing practice.

---

## Port 2049 - NFS

### Description

Network File System.

### Security Risks

* Exposed file shares
* Weak export permissions
* Unauthorized access

---

## Port 2121 - ProFTPD 1.3.1

### Description

Alternative FTP server.

### Security Risks

* Anonymous authentication
* Weak credentials
* File manipulation

### Known Vulnerabilities

Older ProFTPD versions have multiple vulnerabilities, including authentication bypasses and remote code execution depending on enabled modules.

---

## Port 3306 - MySQL

### Description

Relational database server.

### Security Risks

* Default credentials
* Database enumeration
* Privilege escalation
* Sensitive data exposure

---

## Port 5432 - PostgreSQL

### Description

PostgreSQL database service.

### Security Risks

* Default accounts
* Weak authentication
* Remote database access

---

## Port 5900 - VNC

### Description

Remote desktop protocol.

### Security Risks

* Weak passwords
* No encryption in older implementations
* Session hijacking

---

## Port 6000 - X11

### Description

Linux graphical display server.

### Security Risks

* Keyboard interception
* Screen capture
* Unauthorized graphical access

---

## Port 6667 - UnrealIRCd

### Description

Internet Relay Chat server.

### Known Vulnerability

**CVE-2010-2075**

A malicious backdoor was accidentally distributed in compromised UnrealIRCd releases, allowing remote command execution.

---

## Port 8009 - Apache JServ Protocol (AJP)

### Description

Connector between Apache HTTP Server and Apache Tomcat.

### Security Risks

* Internal application exposure
* Information disclosure

### Known Vulnerabilities

**Ghostcat (CVE-2020-1938)**

Affected Tomcat installations may expose application files or allow remote code execution under certain configurations.

---

## Port 8180 - Apache Tomcat

### Description

Java Servlet container.

### Security Risks

* Default credentials
* Manager application exposure
* WAR file deployment
* Remote Code Execution when misconfigured

---

# Security Findings

The target exposes numerous outdated services intentionally included in Metasploitable for penetration-testing practice.

Main findings include:

* 23 open TCP ports
* Multiple obsolete protocols
* Legacy software versions
* Services vulnerable to publicly documented CVEs
* Several opportunities for enumeration and privilege escalation

The host presents a deliberately large attack surface suitable for cybersecurity training.

---

# Packet Capture

A packet capture (`kali-metas.pcapng`) was generated during the assessment.

This file can be opened using **Wireshark** to inspect:

* TCP three-way handshakes
* SYN packets
* Service banners
* TCP responses
* Network communication generated during the scan

---

# Key Takeaways

This exercise provided practical experience with:

* Network reconnaissance
* TCP SYN scanning
* Service fingerprinting
* Operating system detection
* Attack surface identification
* Vulnerability research
* Basic defensive analysis

---

# References

* Nmap Documentation
* OWASP Testing Guide
* MITRE CVE Database
* NIST National Vulnerability Database
* Metasploitable 2 Documentation

---

# Ethical Disclaimer

This repository was created exclusively for educational purposes.

All scans were conducted in an isolated laboratory environment using intentionally vulnerable systems.

No unauthorized systems or third-party networks were scanned or targeted.

The techniques demonstrated here should only be used in environments where explicit permission has been granted.
