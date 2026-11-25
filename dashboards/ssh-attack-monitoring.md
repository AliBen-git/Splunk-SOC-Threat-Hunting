# SSH Attack Monitoring Dashboard

## Project context

This document describes an SSH security-monitoring dashboard built in Splunk Enterprise as part of a SOC analyst portfolio project. The dashboard was created to explore authentication activity, identify repeated SSH failures, and support investigation of potentially suspicious source addresses and targeted account names.

All organization-specific identifiers have been anonymized. This document is a sanitized portfolio artifact and does not contain raw production logs.

## Dashboard overview

The dashboard contains four visualizations:

1. **Top Attacking IPs** – ranks source addresses associated with SSH authentication attempts.
2. **Attack Type Distribution** – displays the distribution of activity classified as brute-force attempts and user-enumeration-like activity.
3. **Attack Activity Over Time** – shows changes in observed SSH activity over the analyzed period.
4. **Most Targeted Usernames** – highlights account names most frequently present in the observed attempts. Account names have been replaced with generic labels.

A separate single-value panel reports **61 successful authentication events** in the dataset.

## Key observations from the analyzed data

- One source address (shown here as **External IP A**) was associated with **343 failed attempts** targeting **16 usernames**.
- Another source address (**External IP B**) was associated with **115 attempts** targeting **71 usernames**. The broad range of targeted usernames is consistent with user-enumeration-like behavior, but does not by itself prove the attacker's intent.
- The dashboard recorded **61 successful authentication events** during the selected data period.
- The activity-over-time chart shows repeated SSH activity and several noticeable peaks during the displayed period.

The original dashboard's source addresses and account names are intentionally not reproduced in this public version.

## Interpretation and investigation notes

Repeated authentication failures can indicate password guessing or brute-force activity. Attempts against many different usernames can indicate username discovery or enumeration. These are behavioral indicators and should be validated against event details and the environment's expected activity.

Successful authentication is not equivalent to confirmed unauthorized access. The 61 events must be reviewed to establish:
- which accounts authenticated;
- whether the accounts and source addresses were expected;
- whether authentication followed suspicious failures;
- whether the sessions performed unusual commands or other post-login actions.

## Data and scope limitations

- The dashboard reflects the events available in the Splunk dataset and the selected time range; it should not be treated as proof of complete network visibility.
- The UFW log analysis did not show SSH port 22 events in the reviewed data; web-port activity such as ports 80 and 443 was visible. This is a logging-visibility limitation, not proof that SSH traffic did not reach the host or that the firewall allowed or blocked it.
- The dashboard's attack classifications depend on the SPL logic and available log fields. They should be validated before operational use.
- Counts represent events in the analyzed dataset, not necessarily unique attacker actions or confirmed compromises.

## Recommended follow-up

1. Validate the 61 successful authentication events against authorized users, expected source addresses, and approved access windows.
2. Correlate suspicious successful logins with preceding failures, Fail2ban events, and available host activity.
3. Review SSH hardening, account permissions, key management, and MFA availability with the system owner.
4. Review UFW logging configuration with an authorized server administrator if network-level SSH visibility is required.
5. Keep time ranges, SPL logic, and data-source limitations documented when sharing dashboard results.

## Privacy note

This public documentation uses generic labels instead of real IP addresses, usernames, hostnames, and organization identifiers. Do not add raw logs, production screenshots, credentials, internal network details, or other confidential information without explicit authorization.
