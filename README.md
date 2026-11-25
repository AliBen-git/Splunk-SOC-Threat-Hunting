# Splunk SOC Threat Hunting & Incident Analysis

**A hands-on SOC analyst portfolio project using Splunk Enterprise to investigate SSH authentication activity, review defensive controls, and communicate security findings.**

This project demonstrates a practical security-monitoring workflow: validating log data, writing SPL searches, identifying suspicious authentication patterns, reviewing Fail2ban and UFW telemetry, building a monitoring dashboard, and documenting investigation results with appropriate caveats.

> **Portfolio note:** This is an independently prepared, anonymised portfolio presentation. Organisation-specific details, usernames, source addresses, hostnames, and raw production log entries are intentionally excluded. The project does not claim that a security compromise was confirmed.

## At a glance

| Area | Description |
|---|---|
| SIEM platform | Splunk Enterprise |
| Query language | Search Processing Language (SPL) |
| Log sources | Linux `auth.log`, `fail2ban.log`, and `ufw.log` |
| Security focus | SSH authentication failures, successful authentications, suspicious source activity, Fail2ban actions, and firewall visibility |
| Deliverables | SPL investigations, dashboard documentation, and an anonymised incident analysis |
| Project type | Practical SOC / cybersecurity portfolio project |

## Project overview

The goal was to use Splunk as a central point for exploring security-relevant logs and supporting a structured investigation of SSH activity.

The work began by checking that the relevant log sources were indexed and that searches returned the expected events. Raw events were then inspected before extracting fields such as usernames, source IP addresses, firewall ports, and security actions. SPL searches were used to summarise authentication patterns and compare observations across the available log sources.

The investigation also highlighted an important operational lesson: a detection is only as reliable as the underlying telemetry and field extraction. Broad IP-address regular expressions initially matched numeric strings that were not valid IP addresses, so the searches were narrowed to the relevant log formats and checked against raw events.

## Investigation highlights

The reviewed dashboard and analysis contained the following observations:

- One source address, anonymised as **External IP A**, was associated with **343 failed authentication attempts** targeting **16 usernames**.
- A second source address, anonymised as **External IP B**, was associated with **115 attempts** involving **71 usernames**. The number of targeted usernames is consistent with possible username-enumeration behaviour, but does not prove intent.
- The dashboard recorded **61 successful authentication events** in the selected dataset and time range. These events require validation against authorised accounts, expected source addresses, and session activity; successful authentication alone does not prove unauthorised access.
- The reviewed UFW data showed events for destination ports **80 and 443**, while SSH port **22** was not present in the examined results. Direct server access was unavailable, so the reason for this visibility gap could not be established.

These are observations from the available dataset, not claims of confirmed compromise or complete network visibility.

## Project objectives

- Explore and validate security log data in Splunk Enterprise.
- Analyse Linux SSH authentication activity, including invalid users, failed passwords, and successful authentications.
- Review Fail2ban detections and actions, including `Found`, `Ban`, and `Unban`.
- Examine UFW events, source addresses, destination ports, and recorded actions.
- Extract and validate fields using SPL and regular expressions.
- Compare evidence across log sources where timestamps and fields support correlation.
- Build dashboard visualisations that communicate security activity.
- Document findings, uncertainty, telemetry limitations, and recommended follow-up.

## Data sources and investigation focus

| Log source | What was investigated |
|---|---|
| `auth.log` | SSH authentication attempts, invalid usernames, failed passwords, and successful authentications |
| `fail2ban.log` | Filter matches and ban/unban actions |
| `ufw.log` | Firewall events, source IP addresses, destination ports, and recorded block/allow indicators |

The Splunk environment also contained Windows Security event data during exploration. The primary use cases documented in this repository focus on the Linux and network-security sources listed above.

## Methodology

1. **Validate ingestion** — confirm that each source is present in Splunk and inspect event volume and time range.
2. **Inspect raw events** — examine representative events before relying on extracted fields.
3. **Extract and validate fields** — use SPL and regular expressions to extract relevant values, then verify them against the original event text.
4. **Investigate authentication patterns** — summarise failed attempts, successful authentications, source addresses, and targeted usernames.
5. **Review defensive telemetry** — examine Fail2ban detections and actions, and determine which firewall ports are visible in UFW logs.
6. **Correlate cautiously** — compare addresses and timestamps across sources without assuming that similar events prove a shared cause.
7. **Assess and communicate** — distinguish observed facts from hypotheses, record data limitations, and recommend validation steps.

## Dashboard

The SSH monitoring dashboard is documented in:

- [SSH Attack Monitoring Dashboard](dashboards/ssh-attack-monitoring.md)

The dashboard documentation describes the following panels:

- Top Attacking IPs
- Attack Type Distribution
- Attack Activity Over Time
- Most Targeted Usernames
- Successful authentication event count

Sanitised screenshots will be added after they have been reviewed to ensure that no organisation-specific or sensitive information is visible.

## Incident analysis

The anonymised investigation write-up is available here:

- [SSH Authentication Activity – Incident Analysis](incident-reports/ssh-authentication-incident-analysis.md)

The report records the observed activity, explains the limits of the available evidence, and recommends follow-up validation by the authorised system owner.

**Assessment:** Suspicious SSH activity was identified. Potential unauthorised access requires further investigation. The evidence currently documented here does not establish that an account was compromised.

## Key lessons learned

- Successful authentication and unauthorised access are different findings and must not be conflated.
- Fail2ban `Found` events are not the same as a confirmed `Ban`; actions should be reviewed separately.
- Missing firewall events are a telemetry or visibility question until logging and firewall configuration can be checked.
- Field extractions must be scoped to the correct log format and validated against raw events.
- Correlation should be based on matching evidence and timestamps, not just similar-looking IP addresses.
- Good SOC reporting communicates uncertainty and limitations as clearly as detections.

## Tools and technologies

- Splunk Enterprise
- Search Processing Language (SPL)
- Linux security logs
- Fail2ban
- UFW (Uncomplicated Firewall)
- Git and GitHub

## Repository structure

```text
.
├── README.md
├── dashboards/
│   └── ssh-attack-monitoring.md
├── incident-reports/
│   └── ssh-authentication-incident-analysis.md
├── spl-queries/          # SPL examples will be added
└── documentation/        # Methodology and supporting notes
```

## Security, privacy, and responsible disclosure

This is a public portfolio project. Do not commit raw production logs, credentials, tokens, private keys, personal data, internal hostnames, private network details, or confidential company information.

Review and anonymise screenshots, event samples, IP addresses, usernames, organisation references, and incident details before publication. Only publish material that you are authorised to disclose.

## Disclaimer

This repository is intended for educational and professional portfolio purposes. The analysis reflects only the log data and access available during the investigation. It is not an official incident report for any organisation, and it does not assert confirmed compromise unless independently validated.

## Author

**AliBen-git**  
Cybersecurity / SOC Analyst portfolio project
