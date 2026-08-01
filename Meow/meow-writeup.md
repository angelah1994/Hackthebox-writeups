# Hack The Box — Meow Writeup

## Table of Contents
1. [Overview](#overview)
2. [Goal of the Machine](#goal-of-the-machine)
3. [Machine Information](#machine-information)
4. [Tools Used](#tools-used)
5. [Host Discovery](#host-discovery)
6. [Enumeration](#enumeration)
7. [Vulnerability Analysis](#vulnerability-analysis)
8. [Exploitation](#exploitation)
9. [Post Exploitation](#post-exploitation)
10. [Skills Demonstrated](#skills-demonstrated)
11. [Mitigation](#mitigation)
12. [Lessons Learned](#lessons-learned)

## Overview

**Meow** is the first machine in the Hack The Box Starting Point series. It is designed to introduce beginners to basic penetration testing concepts including host discovery, service enumeration, remote access, and flag retrieval. The machine exposes an insecure Telnet service that allows unauthenticated access using the default root account. Exploiting this misconfiguration grants full administrative access to the system.

- **Difficulty:** Easy
- **Platform:** Hack The Box — Starting Point
- **Operating System:** Linux

## Goal of the Machine

The objective of this lab is to:
- Discover the target host
- Enumerate exposed services
- Identify insecure configurations
- Gain remote shell access
- Locate and retrieve the user flag

## Tools Used

- Nmap
- Telnet
- Linux Terminal

## Host Discovery

Before performing any enumeration, the target machine was verified to be reachable using the `ping` utility.

<img width="975" height="385" alt="image" src="https://github.com/user-attachments/assets/b12ce398-0231-47b8-a118-440474bf0407" />


After confirming that the target was reachable, an Nmap scan was performed to identify open ports, running services, and service versions. The Nmap scan revealed that only one TCP port was open.

The service was identified as **Telnet**, a legacy remote administration protocol that transmits all communications in plaintext.

No other services were exposed. The presence of Telnet suggested that the machine was intentionally vulnerable to weak authentication or default credentials.

<img width="975" height="310" alt="image" src="https://github.com/user-attachments/assets/0c47f1f4-54ed-4e2d-8a41-76437b123086" />


## Vulnerability Analysis

The primary security issue was the exposure of the Telnet service. Telnet suffers from several major security weaknesses:

- No encryption
- Credentials transmitted in plaintext
- Vulnerable to packet sniffing
- Often configured with weak or default credentials

During authentication testing, the service accepted the **root** account without requiring a password. This resulted in immediate administrative access to the system, representing a critical authentication misconfiguration.

## Exploitation

After identifying Telnet as the only exposed service, a connection was established to the target machine. The Telnet service presented a login prompt, where the username `root` was entered. No password was requested, and the system immediately granted access to a root shell.

This behavior indicated that the server was configured to allow unrestricted root logins without authentication — a critical security misconfiguration. Because root is the highest-privileged account on a Linux system, successful authentication provided complete administrative control over the machine, eliminating the need for any privilege escalation before proceeding to locate and retrieve the flag.

<img width="975" height="423" alt="image" src="https://github.com/user-attachments/assets/619e947c-2a20-47ab-abc8-6615447b30f0" />


<img width="975" height="388" alt="image" src="https://github.com/user-attachments/assets/148e6385-f657-490d-99e1-cc269c7ee09a" />

## Post Exploitation

Upon gaining access to the system, it was confirmed that the session was running with root privileges, meaning no privilege escalation techniques were required. The contents of the current directory were examined to identify any files of interest, revealing the presence of the `flag.txt` file. The file was opened to display its contents, successfully retrieving the machine's flag.

This confirmed that the objective of the lab had been achieved and demonstrated how an insecurely configured Telnet service could lead directly to complete system compromise.

<img width="975" height="388" alt="image" src="https://github.com/user-attachments/assets/d71363d9-909d-4b36-b613-afc24d16663f" />

## Skills Demonstrated

- Network reconnaissance
- Host discovery
- Service enumeration
- Nmap scanning
- Telnet usage
- Authentication testing
- Linux command-line navigation
- Flag retrieval

## Mitigation

The vulnerability demonstrated in this lab can be prevented through several security best practices:

**Disable Telnet**
Telnet should not be used for remote administration because it does not encrypt network traffic.

**Use SSH Instead**
Replace Telnet with Secure Shell (SSH), which provides encrypted authentication and communication.

**Disable Root Remote Login**
Root should never be allowed to authenticate directly over remote management protocols. Administrators should authenticate using standard user accounts and elevate privileges using sudo.

**Enforce Strong Authentication**
- Require strong passwords
- Disable blank passwords
- Implement multi-factor authentication where possible

**Restrict Administrative Services**
Remote management services should only be accessible from trusted IP addresses using firewalls or VPNs.

**Perform Regular Security Audits**
Routine vulnerability scans and configuration reviews help identify insecure services before attackers do.

## Lessons Learned

This machine demonstrates that even a single insecure service can lead to complete system compromise. Leaving Telnet enabled with unrestricted root access effectively removes all authentication controls and highlights the importance of secure remote administration practices.
