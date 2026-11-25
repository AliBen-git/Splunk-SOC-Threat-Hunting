# SSH Authentication Activity – Incident Analysis

## 1. Executive summary

Splunk analysis of SSH authentication logs identified repeated failed login attempts from multiple external source addresses, activity consistent with brute-force attempts and username enumeration, and **61 successful authentication events** in the analyzed dataset.

The available evidence indicates that the host was targeted and that successful SSH authentications occurred. However, the log evidence provided for this report does **not independently establish that those successful authentications were unauthorized or that an account was compromised**. The successful events require validation by the system owner.

**Assessment:** Suspicious SSH activity identified; potential unauthorized access requires further investigation.

## 2. Scope and data sources

The analysis used the following log sources available in Splunk:

- `auth.log` – SSH authentication activity, including failed attempts and successful authentication events.
- `fail2ban.log` – Fail2ban detection and ban/unban activity.
- `ufw.log` – firewall events available in the ingested dataset.

The analysis was performed on an exported dataset covering the period represented in the original dashboard. The dashboard displayed activity from 8–13 March 2026 and was generated on 21 March 2026.

## 3. Detection approach

Splunk Search Processing Language (SPL) was used to:

- identify repeated failed SSH authentication attempts;
- extract source IP addresses and targeted usernames from relevant events;
- identify successful authentication events, including `Accepted password` and `Accepted publickey` messages;
- summarize activity by source address and username;
- compare SSH-related findings with Fail2ban and UFW events.

## 4. Key findings

### 4.1 Repeated failed authentication

- **External IP A:** 343 failed attempts targeting 16 usernames.
- **External IP B:** 115 attempts across 71 usernames.

The high volume of attempts and the broad set of targeted usernames are consistent with automated password-guessing and username-enumeration behavior. These patterns are indicators and should be assessed in context.

### 4.2 Successful authentication events

The dashboard recorded **61 successful authentication events**.

The source report also noted that at least one source address associated with failed-attempt activity appeared in successful-authentication records. This overlap warrants investigation. It does not, on its own, prove that the successful login was performed by the same attacker or that access was unauthorized.

### 4.3 Fail2ban and firewall visibility

Fail2ban logs were used to review detection and ban-related activity.

In the reviewed UFW data, events for ports 80 and 443 were visible, while SSH port 22 events were not observed. This is a limitation in the available firewall-log evidence. Without access to the server configuration and logging settings, it is not possible to determine why SSH events are absent or to conclude whether SSH traffic was allowed or blocked by UFW.

## 5. Risk and potential impact

**Provisional risk: Elevated – investigation required.**

Potential consequences, if unauthorized access is confirmed, could include:
- unauthorized use of an account;
- access to sensitive information;
- execution of commands or changes to the host;
- persistence or additional access to other systems.

These are potential impacts, not impacts established by the evidence reviewed for this report.

## 6. Recommended response actions

The following actions should be performed by the authorized system owner or incident-response team.

### Validation and investigation

1. Identify the account, source address, timestamp, and authentication method for each of the 61 successful events.
2. Confirm whether the accounts and source addresses were authorized and expected.
3. Correlate successful events with preceding failed attempts and Fail2ban records.
4. Review session activity, command history, authentication keys, account changes, scheduled tasks, and other relevant host telemetry.
5. Preserve relevant logs and timestamps for further investigation.

### Containment, if unauthorized access is confirmed or suspected by the incident owner

1. Follow the organization's incident-response process to contain suspicious sessions or accounts.
2. Review and, where appropriate, revoke exposed credentials or SSH keys.
3. Apply access restrictions and block source addresses only after validation and approval.
4. Avoid disruptive changes to production access without coordination with the system owner.

### Security improvements

- Prefer SSH key-based authentication and disable password authentication where operationally appropriate.
- Enforce MFA where supported.
- Apply least-privilege access and review privileged accounts.
- Maintain Fail2ban or an equivalent protective control with monitored configuration.
- Centralize and retain authentication, firewall, and relevant host activity logs.
- Review firewall logging coverage for security-relevant traffic, including SSH, with the server administrator.

## 7. Conclusion

The Splunk investigation identified substantial suspicious SSH authentication activity, including repeated failures, username-enumeration-like behavior, and 61 successful authentication events. These findings justify further investigation by the system owner.

Based on the evidence available in the supplied dashboard and report, unauthorized access or account compromise cannot yet be stated as confirmed. The next critical step is to validate the successful authentication events and investigate the associated sessions.

## 8. Public portfolio and privacy statement

This report is a sanitized portfolio version. Real source IP addresses, usernames, hostnames, and organization-specific identifiers have been replaced or omitted. It contains no raw production log entries. The document is intended to demonstrate a security-analysis workflow and should not be interpreted as an official incident report issued by the organization.
