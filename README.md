# Cloud-Account-Takeover-Investigation
SOC investigation of a cloud account takeover involving password spraying, compromised accounts, MFA persistence, malicious mailbox rules, and KQL log analysis.

# Cloud Account Takeover Investigation

Overview

In this project, I investigated a Microsoft 365 account takeover involving a password-spray attack. I analyzed sign-in and audit logs to identify how the attacker gained access, what actions they performed, which accounts were affected, and how the incident could be contained.

##Tools Used
KQL
Azure Data Explorer: This investigation was performed using Azure Data Explorer. The same KQL syntax is used in Microsoft Sentinel for log analysis and threat hunting.
Entra ID Sign-in Logs
Microsoft 365 Audit Logs
MITRE ATT&CK
Investigation

I used KQL to analyze authentication and audit activity across the environment. I investigated repeated failed login attempts, suspicious successful logins, MFA changes, mailbox activity, and access to cloud resources.

Key Findings
I identified password-spray activity against multiple accounts.
I confirmed the CEO’s account was compromised.
I identified an unauthorized MFA device added to the CEO’s account.
I discovered a malicious mailbox rule hiding financial emails.
I identified a second compromised account.
I found that the second compromised account was used to access SharePoint Online.
MITRE ATT&CK Mapping

I mapped the attacker’s activity to relevant MITRE ATT&CK techniques, including:

T1110.003 – Password Spraying
T1078 – Valid Accounts
T1098.005 – Device Registration
T1114.002 – Remote Email Collection
T1213.002 – SharePoint
Incident Response

I documented containment actions including revoking active sessions and tokens, resetting credentials and MFA, removing the unauthorized MFA device and malicious mailbox rule, blocking attacker IP addresses, and reviewing other targeted accounts for compromise.

Full Incident Report

My full incident report contains the investigation timeline, findings, indicators of compromise, MITRE ATT&CK mapping, scope, response actions, recommendations, and lessons learned.
