# MITRE ATT&CK Mapping

I mapped the activity identified during my investigation to the MITRE ATT&CK framework. This helped me connect the evidence from the logs to known attacker techniques and understand how the attack progressed.

## T1110.003 – Password Spraying

**Tactic:** Credential Access

**Evidence:** I identified repeated failed authentication attempts against multiple Cloudora accounts from the same `102.89.x.x` attacker infrastructure. The activity occurred over multiple days and included repeated `ResultType 50126` authentication failures.

**Why I mapped it:** The attacker attempted authentication across multiple accounts before successful account access, which was consistent with password-spraying behavior.

---

## T1078 – Valid Accounts

**Tactic:** Initial Access

**Evidence:** I identified successful authentications to Daniel Reeve and Priya Nair from the suspicious Lagos-based attacker infrastructure.

**Why I mapped it:** The attacker successfully authenticated using legitimate Cloudora user accounts.

---

## T1098.005 – Device Registration

**Tactic:** Persistence

**Evidence:** Shortly after Daniel's account was compromised, an Authenticator method associated with a **Pixel 6** was registered from the attacker infrastructure.

**Why I mapped it:** Registering an additional authentication device provided a method for maintaining access to the compromised account.

---

## T1114.002 – Remote Email Collection

**Tactic:** Collection

**Evidence:** After compromising Daniel's account, the attacker successfully accessed **Outlook Web** from the suspicious Lagos-based IP address.

**Why I mapped it:** The compromised account was used to remotely access the CEO's cloud-hosted mailbox.

---

## T1213.002 – SharePoint

**Tactic:** Collection

**Evidence:** After Priya Nair's account was compromised, I identified successful access to **SharePoint Online** from the attacker infrastructure.

**Why I mapped it:** The compromised account was used to access organizational information stored within SharePoint.

---

## T1070.008 – Clear Mailbox Data

**Tactic:** Defense Evasion

**Evidence:** I identified a malicious mailbox rule named **"RSS Subscriptions"** that targeted financial and invoice-related emails, moved them to RSS Feeds, and marked them as read.

**Why I mapped it:** The mailbox manipulation was used to conceal financial communications from the CEO and reduce the likelihood that the activity would be noticed.

---

## Attack Flow

**Password Spraying → Valid Account Access → MFA Device Registration → Email Access → Mailbox Manipulation → Second Account Compromise → SharePoint Access**

Mapping the investigation to MITRE ATT&CK helped me understand how individual log events connected together as part of a larger attack chain rather than treating each event as an isolated alert.
