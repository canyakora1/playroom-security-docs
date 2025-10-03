# Active Directory Attacks

This section covers various attacks that can be performed against Active Directory (AD) environments. AD is a widely used directory service in Windows-based networks, and understanding these attacks is crucial for securing such environments.
## Table of Contents
- [Introduction to Active Directory Attacks](#introduction-to-active-directory-attacks)
- [Common Active Directory Attacks](#common-active-directory-attacks)
- [Detection and Mitigation Strategies](#detection-and-mitigation-strategies)
- [Conclusion](#conclusion)
- [References](#references)

## Introduction to Active Directory Attacks
Active Directory (AD) is a critical component of many enterprise networks, providing authentication and authorization services. Due to its central role, AD is a prime target for attackers seeking to gain unauthorized access or escalate privileges within a network. This document explores common AD attacks, their methodologies, and strategies for detection and mitigation.
## Common Active Directory Attacks
### 1. Pass-the-Hash (PtH)
Pass-the-Hash is an attack technique where an attacker captures the hashed password of a user and uses it to authenticate without needing to know the actual password. This is often done by extracting hashes from memory or the SAM database.
### 2. Kerberoasting
Kerberoasting involves requesting service tickets for service accounts in AD and then attempting to crack the ticket offline to retrieve the plaintext password. This attack targets service accounts that often have elevated privileges.
### 3. Golden Ticket Attack
A Golden Ticket attack involves creating a forged Kerberos Ticket Granting Ticket (TGT) using the KRBTGT account's hash. This allows attackers to impersonate any user in the domain, including domain administrators.
### 4. Silver Ticket Attack
Similar to the Golden Ticket attack, the Silver Ticket attack involves forging service tickets (TGS) for specific services. This attack is less powerful than the Golden Ticket but can still provide access to targeted services.
### 5. DCSync Attack
DCSync is an attack that allows an attacker to simulate the behavior of a domain controller and request password hashes and other sensitive information from AD. This is typically done using tools like Mimikatz.
### 6. LDAP Injection
LDAP Injection is a technique where an attacker exploits vulnerabilities in applications that interact with AD via LDAP queries. By injecting malicious LDAP statements, attackers can manipulate queries to gain unauthorized access or extract sensitive information.
### 7. SID History Injection
SID History Injection involves adding a malicious Security Identifier (SID) to a user's SID history attribute. This can allow the user to inherit permissions from another account, potentially escalating privileges.
## Detection and Mitigation Strategies
### Detection
- Monitor for unusual authentication patterns, such as multiple failed login attempts or logins from unexpected locations.
- Use Security Information and Event Management (SIEM) systems to analyze logs for signs of PtH, Kerberoasting, and other AD attacks.
- Implement honeypots to detect unauthorized access attempts.
### Mitigation
- Enforce strong password policies and use multi-factor authentication (MFA) to reduce the risk of credential theft.
- Regularly update and patch AD servers and related infrastructure to mitigate vulnerabilities.
- Limit the use of privileged accounts and implement the principle of least privilege.              