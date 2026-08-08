# KQL Queries

Below are the KQL queries I used during my Cloud Account Takeover investigation. I used these queries to investigate authentication activity, identify suspicious IP addresses, analyze compromised accounts, and review audit activity.

## 1. Review All Sign-In Activity

```kusto
CloudoraSignIn_CL
| order by TimeGenerated asc
```

Used to review the overall sign-in activity and establish a timeline of events.

## 2. Identify Failed Authentication Attempts

```kusto
CloudoraSignIn_CL
| where ResultType == 50126
| project TimeGenerated, UserPrincipalName, IPAddress, Country, City, ResultType
| order by TimeGenerated asc
```

Used to identify repeated failed authentication attempts associated with the password-spray activity.

## 3. Identify Successful Authentications

```kusto
CloudoraSignIn_CL
| where ResultType == 0
| project TimeGenerated, UserPrincipalName, IPAddress, Country, City, AppDisplayName, ResultType
| order by TimeGenerated asc
```

Used to identify successful logins and compare them with the earlier failed authentication attempts.

## 4. Investigate CEO Sign-In Activity

```kusto
CloudoraSignIn_CL
| where UserPrincipalName == "daniel.reeve@cloudora.io"
| summarize SignIns=count(),
          FirstSeen=min(TimeGenerated),
          LastSeen=max(TimeGenerated)
          by Country, City, IPAddress
| order by SignIns desc
```

Used to identify the locations and IP addresses associated with Daniel Reeve's account.

## 5. Review CEO Sign-In Timeline

```kusto
CloudoraSignIn_CL
| where UserPrincipalName == "daniel.reeve@cloudora.io"
| project TimeGenerated, UserPrincipalName, IPAddress,
          Country, City, AppDisplayName, ResultType
| order by TimeGenerated asc
```

Used to review Daniel's authentication activity chronologically and identify the transition from failed attempts to successful attacker access.

## 6. Investigate Priya Nair's Account

```kusto
CloudoraSignIn_CL
| where UserPrincipalName == "priya.nair@cloudora.io"
| project TimeGenerated, UserPrincipalName, IPAddress,
          Country, City, AppDisplayName, ResultType
| order by TimeGenerated asc
```

Used to identify suspicious authentication activity against Priya's account and the successful compromise.

## 7. Investigate Primary Attacker IP

```kusto
CloudoraSignIn_CL
| where IPAddress == "102.89.44.17"
| project TimeGenerated, UserPrincipalName, IPAddress,
          Country, City, AppDisplayName, ResultType
| order by TimeGenerated asc
```

Used to investigate activity originating from the IP address associated with the CEO compromise.

## 8. Investigate All Identified Attacker IPs

```kusto
CloudoraSignIn_CL
| where IPAddress in ("102.89.44.17", "102.89.44.23", "102.89.45.101")
| project TimeGenerated, UserPrincipalName, IPAddress,
          Country, City, AppDisplayName, ResultType
| order by TimeGenerated asc
```

Used to review activity originating from the attacker infrastructure across the environment.

## 9. Identify Accounts Targeted by Attacker Infrastructure

```kusto
CloudoraSignIn_CL
| where IPAddress in ("102.89.44.17", "102.89.44.23", "102.89.45.101")
| summarize Attempts=count(),
          FirstSeen=min(TimeGenerated),
          LastSeen=max(TimeGenerated)
          by UserPrincipalName, IPAddress, ResultType
| order by FirstSeen asc
```

Used to determine which Cloudora accounts were targeted and help establish the scope of the password-spray attack.

## 10. Identify Successful Logins From Attacker IPs

```kusto
CloudoraSignIn_CL
| where IPAddress in ("102.89.44.17", "102.89.44.23", "102.89.45.101")
| where ResultType == 0
| project TimeGenerated, UserPrincipalName, IPAddress,
          Country, City, AppDisplayName, ResultType
| order by TimeGenerated asc
```

Used to identify which targeted accounts were successfully accessed from the attacker infrastructure.

## 11. Review CEO Audit Activity

```kusto
CloudoraAudit_CL
| where UserPrincipalName == "daniel.reeve@cloudora.io"
| project TimeGenerated, UserPrincipalName, Operation, IPAddress
| order by TimeGenerated asc
```

Used to investigate actions performed after Daniel's account was compromised, including changes made to the account and mailbox.

## 12. Review Audit Activity From Attacker IP

```kusto
CloudoraAudit_CL
| where IPAddress == "102.89.44.17"
| project TimeGenerated, UserPrincipalName, Operation, IPAddress
| order by TimeGenerated asc
```

Used to identify account and mailbox changes performed from the IP address associated with the CEO compromise.

## Investigation Summary

Using these queries, I was able to identify:

* Password-spray activity against multiple Cloudora accounts
* Suspicious authentication activity originating from Lagos, Nigeria
* Successful compromise of the CEO's account
* Unauthorized access to Outlook Web and Azure Portal
* Registration of an unauthorized MFA device
* Creation of a malicious mailbox rule hiding financial emails
* Compromise of a second user account
* Unauthorized SharePoint Online access
* The overall scope of accounts targeted by the attacker
