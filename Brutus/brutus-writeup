# Brutus — Brute Force Log Analysis

## Table of Contents
1. [Overview](#overview)
2. [Goal of the Investigation](#goal-of-the-investigation)
3. [Tools Used](#tools-used)
4. [Investigation Steps](#investigation-steps)
5. [Findings](#findings)
6. [Indicators of Compromise](#indicators-of-compromise)
7. [Skills Demonstrated](#skills-demonstrated)
8. [Mitigation](#mitigation)
9. [Conclusion](#conclusion)

## Overview

This writeup documents the analysis of SSH authentication logs (`auth.log`) to identify signs of a brute force attack against a target Linux system.

## Goal of the Investigation

The objective of this analysis was to:
- Extract and review the provided log evidence
- Identify patterns consistent with brute force login attempts
- Determine the source IP and account(s) targeted
- Establish a timeline of failed and successful authentication events
- Draw conclusions to support incident response and mitigation

## Tools Used

- Linux Terminal
- 7-Zip (`7z`) — for extracting the password-protected evidence archive
- `grep` — for filtering log entries
- `utmp.py` — a Python-based UTMP parser used to inspect the `wtmp` login records
- Authentication log file (`auth.log`)

## Investigation Steps

The investigation began with a password-protected archive, `Brutus.zip`, containing the log evidence. The archive was extracted using 7-Zip.

<img width="975" height="190" alt="image" src="https://github.com/user-attachments/assets/64fe8978-02aa-4295-b866-1d787333ea30" />
<img width="975" height="669" alt="image" src="https://github.com/user-attachments/assets/153dde0a-1905-4b56-b9e9-d07f09bac10b" />


The archive contained three files: `auth.log`, `wtmp`, and `utmp.py`. The `auth.log` file was reviewed first using `cat` to get an overview of system activity.

<img width="975" height="398" alt="image" src="https://github.com/user-attachments/assets/de3c8e6a-41c1-42df-a793-719d173a24b6" />


The log showed routine cron session activity for the `confluence` user, followed by an SSH login: a password was accepted for the `root` account from IP `203.101.190.9`, establishing a baseline of legitimate administrative access on the system.

An attempt was made to read the `wtmp` binary login record file using `last -f wtmp`, but this failed due to a file format error. The included `utmp.py` script (a custom UTMP parser) was reviewed as an alternative method for parsing the login records.

<img width="975" height="591" alt="image" src="https://github.com/user-attachments/assets/106f1b7d-5d50-4c25-9c55-133a47616b6e" />

<img width="925" height="251" alt="image" src="https://github.com/user-attachments/assets/1f7e2dc2-1aba-4bc5-aceb-1453d3d6f6a7" />


## Findings

Filtering `auth.log` for accepted passwords originating from IP `65.2.161.68` revealed multiple successful logins using this address — including one for the `root` account and one for a secondary account, `cyberjunkie`.

<img width="975" height="126" alt="image" src="https://github.com/user-attachments/assets/376ef2ac-133b-43da-8fcb-4d1f8b11309f" />
<img width="975" height="139" alt="image" src="https://github.com/user-attachments/assets/bc9c03be-e7a6-4bcd-bf16-3f5afb09167e" />



Separately, filtering the log for `"invalid user"` entries revealed a large volume of failed login attempts from the same IP address, `65.2.161.68`, cycling through multiple usernames (including `admin` and `server_adm`) in rapid succession — a pattern consistent with automated brute force/credential-guessing behavior.

<img width="975" height="435" alt="image" src="https://github.com/user-attachments/assets/16568e99-ecf6-4811-a239-0a7637fbb695" />
<img width="975" height="145" alt="image" src="https://github.com/user-attachments/assets/00f5d9df-b6b1-4bdc-ba1c-1811ff682f9b" />
<img width="975" height="145" alt="image" src="https://github.com/user-attachments/assets/78c75ba9-9a6f-4c1e-8198-9417abe5d989" />
<img width="975" height="198" alt="image" src="https://github.com/user-attachments/assets/d9a2df66-a45b-4a7b-bc7d-66ed554de386" />
<img width="975" height="248" alt="image" src="https://github.com/user-attachments/assets/eced9551-8615-4e0b-b46b-beab3bad3816" />
<img width="975" height="200" alt="image" src="https://github.com/user-attachments/assets/d330f838-04f3-4a61-afff-1b5603238a33" />


## Indicators of Compromise

- **Source IP:** `65.2.161.68`
- **Behavior:** High-volume failed login attempts cycling through multiple usernames (`admin`, `server_adm`, and others) in rapid succession
- **Outcome:** Successful logins from the same IP for the `root` account and a `cyberjunkie` account shortly after the failed attempts
- **Legitimate baseline access:** A separate accepted login for `root` from `203.101.190.9`, unrelated to the attacking IP

## Skills Demonstrated

- Extraction and handling of password-protected log evidence
- Linux log file analysis
- Use of `grep` for targeted log filtering
- Reviewing and reasoning about UTMP/WTMP login records
- Pattern recognition in authentication events
- Brute force attack identification
- Incident timeline reconstruction

## Mitigation

- Implement account lockout policies after a threshold of failed login attempts
- Enable rate limiting on SSH authentication endpoints
- Monitor and alert on repeated failed logins from a single source IP
- Enforce strong password policies and multi-factor authentication
- Block or restrict SSH access from IPs showing brute force behavior
- Disable direct root login over SSH and require privilege escalation via sudo

## Conclusion

The combination of a high volume of failed login attempts across multiple usernames, followed by successful logins to both the `root` and `cyberjunkie` accounts from the same source IP (`65.2.161.68`), strongly indicates a successful brute force attack. This highlights the importance of monitoring authentication logs for repeated failures and unusual login timing patterns to detect credential-based attacks early.
