# Cloud Account Takeover Investigation

SOC investigation of a cloud account takeover involving password spraying, compromised accounts, MFA persistence, malicious mailbox rules, and KQL log analysis.

## Overview

In this project, I investigated a Microsoft 365 account takeover involving a password-spray attack. I analyzed sign-in and audit logs to determine how the attacker gained access, what actions they performed after gaining access, which accounts were affected, and how the incident could be contained.

## Tools Used

- Kusto Query Language (KQL)
- Azure Data Explorer
- Entra ID Sign-in Logs
- Microsoft 365 Audit Logs
- MITRE ATT&CK

> This investigation was performed using Azure Data Explorer. The same KQL syntax is used in Microsoft Sentinel for log analysis and threat hunting.

## Investigation

I used KQL to analyze authentication and audit activity across the environment. I investigated failed login attempts, suspicious successful logins, MFA changes, mailbox activity, and access to cloud resources.

## Key Findings

- Identified password-spray activity targeting multiple accounts.
- Confirmed the compromise of the CEO's account.
- Identified an unauthorized MFA device added to the CEO's account.
- Discovered a malicious mailbox rule designed to hide financial emails.
- Identified a second compromised account.
- Confirmed unauthorized access to SharePoint Online.

## MITRE ATT&CK Mapping

The attacker's activity was mapped to the following MITRE ATT&CK techniques:

- T1110.003 – Password Spraying
- T1078 – Valid Accounts
- T1098.005 – Device Registration
- T1564.008 – Email Hiding Rules
- T1114.002 – Remote Email Collection
- T1213.002 – SharePoint

## Incident Response

I documented the following containment actions:

- Revoked active sessions and tokens.
- Reset compromised credentials.
- Reset MFA configurations.
- Removed the unauthorized MFA device.
- Removed the malicious mailbox rule.
- Blocked attacker IP addresses.
- Reviewed other targeted accounts for additional signs of compromise.

## Repository Contents

- Incident Report
- KQL Queries
- MITRE ATT&CK Mapping
- Indicators of Compromise
- False Positive Analysis
- Evidence and Supporting Documentation

## Full Incident Report

The full incident report includes the investigation timeline, findings, indicators of compromise, MITRE ATT&CK mapping, scope, response actions, recommendations, and lessons learned.
