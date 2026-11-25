# Splunk SOC Threat Hunting

**SIEM monitoring, log analysis, threat detection, and incident investigation with Splunk Enterprise.**

This repository documents a practical SOC analyst portfolio project focused on investigating authentication activity, reviewing defensive controls, and presenting security findings through Splunk searches and dashboards.

> **Project status:** Portfolio documentation in progress. The repository is being built iteratively; only artefacts that have been reviewed and sanitised will be published.

## Project objectives

- Ingest and explore security-relevant log data in Splunk Enterprise.
- Analyse Linux SSH authentication activity and identify failed and successful authentication events.
- Review Fail2ban detections and actions, including `Found`, `Ban`, and `Unban`.
- Examine UFW firewall events, source addresses, destination ports, and recorded actions.
- Correlate evidence across log sources where the available fields and timestamps support it.
- Build dashboards that communicate security activity and investigation findings.
- Document limitations, recommendations, and lessons learned.

## Environment and data sources

| Source | Investigation focus |
|---|---|
| `auth.log` | SSH authentication attempts, invalid users, failed passwords, and successful authentications |
| `fail2ban.log` | Detection events and ban/unban actions |
| `ufw.log` | Firewall events, source IP addresses, destination ports, and block/allow indicators |

The Splunk environment also contained Windows Security event data during exploration. The primary use cases documented here focus on the Linux authentication and network-security logs listed above.

## Investigation workflow

1. **Validate data availability** — confirm the relevant events, source values, time range, and event volume in Splunk.
2. **Inspect raw events** — review representative raw log lines before writing field extractions.
3. **Extract and validate fields** — use SPL and regular expressions to extract fields such as IP addresses, actions, and destination ports; verify the extracted values against the raw events.
4. **Hunt for suspicious activity** — examine repeated authentication failures, unusual source addresses, and Fail2ban activity.
5. **Review firewall telemetry** — identify which ports and actions are actually represented in the collected UFW events.
6. **Correlate evidence carefully** — compare source IPs and timestamps across data sources without assuming that similar events prove the same activity.
7. **Communicate findings** — summarise evidence, uncertainty, operational impact, and practical recommendations.

## Key investigation considerations

- A successful authentication event is not, by itself, proof of unauthorised access. It must be validated against the account owner, expected activity, and relevant context.
- A Fail2ban `Found` event indicates that the filter matched activity; it is not equivalent to a confirmed ban. Review `Ban` and `Unban` events separately.
- UFW logs observed during this investigation showed destination-port activity for ports **80 and 443**. Port 22 was not present in the examined UFW results. Because direct server access was not available, the reason could not be confirmed. This should be reported as a telemetry limitation to investigate, not proof that SSH traffic was allowed or unprotected.
- IP extraction must be scoped to the relevant log format. A broad IPv4-shaped regular expression can incorrectly match version numbers or other numeric strings.

## Repository structure

The repository will be expanded with reviewed artefacts in the following areas:

- `spl-queries/` — documented SPL searches for authentication, Fail2ban, UFW, and threat hunting.
- `dashboards/` — dashboard descriptions, exported dashboard configuration where appropriate, and sanitised screenshots.
- `incident-reports/` — anonymised incident-analysis examples.
- `documentation/` — methodology, architecture, findings, and recommendations.

## Tools and technologies

- Splunk Enterprise
- Search Processing Language (SPL)
- Linux security logs
- Fail2ban
- UFW (Uncomplicated Firewall)
- Git and GitHub

## Security, privacy, and responsible disclosure

This is a portfolio project. No raw production logs, credentials, tokens, private keys, personal data, internal hostnames, or confidential company information should be committed to this public repository.

Any screenshots, event samples, IP addresses, usernames, organisation references, and incident details must be reviewed and anonymised before publication. Only share material that you are authorised to disclose.

## Disclaimer

This repository is for educational and professional portfolio purposes. The analysis reflects the log data and access available during the investigation. Findings are evidence-based and should not be interpreted as confirmation of compromise unless independently validated.

## Author

**AliBen-git**  
SOC Analyst / Cybersecurity portfolio project

---

*More SPL examples, dashboard documentation, and sanitised investigation artefacts will be added as they are reviewed.*
