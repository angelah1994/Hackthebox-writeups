# Brutus — Brute Force Log Analysis

## Table of Contents
1. [Overview](#overview)
2. [Log Analysis](#log-analysis)
3. [Findings](#findings)
4. [Indicators of Compromise](#indicators-of-compromise)
5. [Conclusion](#conclusion)

## Overview

This writeup documents the analysis of authentication logs to identify signs of a brute force attack against a target system.

## Log Analysis

![Log overview](images/brutus-01.png)

![Log entries showing login attempts](images/brutus-02.png)

![Log entries continued](images/brutus-03.png)

![Log entries continued](images/brutus-04.png)

![Log entries continued](images/brutus-05.png)

![Log entries continued](images/brutus-06.png)

![Log entries continued](images/brutus-07.png)

## Findings

There were 4 instances of an accepted password using the same credentials within a short time. This pattern indicated something suspicious going on.

![Accepted password instances](images/brutus-08.png)

With so many invalid users, it was concluded that the person on the machine with IP `65.2.161.68` was trying to brute force the target system.

![Invalid user attempts](images/brutus-09.png)

![Invalid user attempts continued](images/brutus-10.png)

![Invalid user attempts continued](images/brutus-11.png)

![Invalid user attempts continued](images/brutus-12.png)

## Indicators of Compromise

- **Source IP:** `65.2.161.68`
- **Behavior:** Repeated failed login attempts across multiple usernames
- **Outcome:** Same password accepted 4 times within a short time window following the failed attempts

## Conclusion

The combination of numerous invalid login attempts followed by a rapid succession of accepted logins using identical credentials strongly suggests a brute force attack originating from `65.2.161.68`. This highlights the importance of monitoring authentication logs for repeated failures and unusual login timing patterns to detect credential-based attacks early.
