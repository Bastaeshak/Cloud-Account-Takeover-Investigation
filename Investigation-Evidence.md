# Investigation Evidence

## 1. Password Spray Detection

I used KQL to identify repeated failed authentication attempts across multiple Cloudora accounts. The activity showed a pattern consistent with password spraying.


<img width="1906" height="900" alt="password spray" src="https://github.com/user-attachments/assets/866962ad-7e52-4511-9d39-d0fc81a858b7" />


## 2. CEO Account Compromise

I reviewed Daniel Reeve's sign-in activity and identified multiple failed authentication attempts from Lagos, Nigeria, followed by a successful login from the same suspicious infrastructure. Shortly after the successful authentication, the account was used to access Outlook Web and the Azure Portal.

This activity was suspicious because Daniel's normal sign-in activity originated from London using a different device and browser. The failed attempts followed by a successful login also occurred after several days of password-spray activity against Cloudora accounts.

**Indicators of Compromise:**
- IP Address: `102.89.44.17`
- Location: Lagos, Nigeria
- Device: Windows 10 / Chrome 125
- ResultType `50126`: Failed authentication
- ResultType `0`: Successful authentication
- Affected Account: `daniel.reeve@cloudora.io`

**Why it matters:** The successful authentication from the same infrastructure associated with the password-spray activity confirmed that the CEO's account was compromised and allowed the attacker to begin accessing Microsoft 365 resources.


<img width="1898" height="889" alt="02-CEO-Account-Compromise" src="https://github.com/user-attachments/assets/a9a40ce4-e13b-490b-a529-12cd4a6bfe1b" />


## 3. CEO Account Baseline

I created a baseline of Daniel Reeve's sign-in activity to compare his normal behavior with the suspicious login activity. His account showed consistent activity from London across multiple days, while Lagos appeared only during the attack period.

The Lagos activity was suspicious because it came from a new location and IP address and was associated with failed authentication attempts before the successful login. Comparing this activity against Daniel's normal sign-in history helped me determine that the Lagos authentication was not consistent with his usual behavior.

**Indicators of Compromise:**
- Location: Lagos, Nigeria
- Suspicious IP: `102.89.44.17`
- Normal Location: London, United Kingdom

**Why it matters:** Establishing a normal account baseline helped distinguish legitimate activity from the unauthorized attacker activity and provided additional evidence that Daniel's account was compromised.


<img width="1886" height="892" alt="03-CEO-Account-Baseline" src="https://github.com/user-attachments/assets/1aef6804-6442-4ca4-97fb-d5b918e9e8ba" />


## 4. Persistence Activity

I reviewed audit activity from the attacker infrastructure to identify changes made after the account compromise. The logs showed that the attacker registered their own authenticator device, identified as a Pixel 6, and later created a malicious inbox rule named "RSS Subscriptions."

The inbox rule targeted emails from finance or containing the word "invoice," moved them to RSS Feeds, and marked them as read. These actions showed that the attacker was attempting to maintain access and hide financial communications from the CEO.

**Indicators of Compromise:**
- IP Range: `102.89.x.x`
- MFA Device: Pixel 6
- Mailbox Rule: `RSS Subscriptions`
- Activity: `User registered security info`
- Activity: `New inbox rule created`

**Why it matters:** These changes established persistence and concealed financial email activity, meaning a password reset alone would not have fully removed the attacker.


<img width="1906" height="881" alt="04-Persistence-Activity" src="https://github.com/user-attachments/assets/779820e3-3990-418b-a727-b28c3e492143" />


## 5. Scope of Compromised Accounts

I searched for successful authentications originating from the attacker infrastructure to determine whether Daniel was the only compromised user. The results showed successful access to both Daniel Reeve and Priya Nair from the `102.89.x.x` attacker IP range.

This expanded the scope of the incident from a single CEO account takeover to a multi-account compromise.

**Indicators of Compromise:**
- Attacker IP Range: `102.89.x.x`
- Compromised Account: `daniel.reeve@cloudora.io`
- Compromised Account: `priya.nair@cloudora.io`
- ResultType: `0` (Successful authentication)

**Why it matters:** Identifying the second compromised account ensured the investigation and containment actions covered the full known scope of the attack instead of focusing only on the CEO.


<img width="1900" height="898" alt="05-Scope-Compromised-Accounts" src="https://github.com/user-attachments/assets/766502ce-1310-47ad-9e07-94b6b1ff280c" />


## 6. Post-Containment Verification

After containment, I reran the attacker IP query to verify that no additional successful authentications were occurring from the `102.89.x.x` infrastructure.

I used this check to confirm that the response actions were effective and that the attacker was no longer actively accessing the compromised accounts.

**Why it matters:** Verifying containment is important because response actions are not complete until the SOC confirms that malicious access has stopped.


<img width="1913" height="899" alt="06-Post-Containment-Verification" src="https://github.com/user-attachments/assets/c3e3b713-b245-43c2-9229-83c4cb12b773" />


