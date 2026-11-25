# SPL Query Notes and Examples

This document turns the author's Splunk learning notes into a structured reference for the SOC threat-hunting portfolio. It preserves the topics and example fields from the original notes while identifying syntax corrections and assumptions that must be checked against the actual events.

> **Important:** SPL examples are templates. Field names, source names, and sourcetypes must match the data in the target Splunk environment. Validate every search against raw events before using its output as incident evidence.

## 1. Boolean operators

Boolean operators combine search conditions:

| Operator | Purpose | Example |
|---|---|---|
| `AND` | Both conditions must match | `failed AND password` |
| `OR` | Either condition may match | `failed OR password` |
| `NOT` | Excludes events matching a condition | `failed NOT password` |

Use parentheses to make the intended logic explicit, especially when combining multiple operators:

```spl
(failed OR authentication) AND login
```

This searches for events containing `login` and either `failed` or `authentication`.

## 2. Basic search structure and the pipe

A search commonly starts by selecting an index and, where appropriate, a sourcetype or host:

```spl
index=main sourcetype=csv host="multi_type_logs"
```

The pipe character (`|`) passes the results of one search stage to the next command. Each command can filter, extract, transform, aggregate, or sort the results.

Example from the learning notes:

```spl
index=main sourcetype=csv auth_action=password_charge auth_result=failed
| stats max(port) AS Highest_port
```

This calculates the highest observed value of `port` among events matching the search conditions. Confirm that `auth_action`, `auth_result`, and `port` are actual fields in the dataset; the field value `password_charge` is retained as written in the original notes and should be checked against raw events.

## 3. Port-scanning events

The original notes include this aggregation:

```spl
index=main sourcetype=csv reason="port scanning detected"
| stats count AS Total_PortScanning
```

It counts matching events and returns one total value. The output is an event count, not necessarily a count of unique scans or unique source addresses.

## 4. Phishing-related events

The original notes contain the following search:

```spl
index=main sourcetype=csv reason="phishing attempt"
| stats count By as source_ip
| where count > 3
| sort - count
```

The `stats` line has invalid syntax. If the intention is to count matching events per source IP and show sources with more than three events, use:

```spl
index=main sourcetype=csv reason="phishing attempt"
| stats count by source_ip
| where count > 3
| sort -count
```

This returns source IP values with more than three matching events. It does **not** return only the overall phishing-event total.

To calculate the total number of matching phishing-related events instead, use:

```spl
index=main sourcetype=csv reason="phishing attempt"
| stats count AS Total_Phishing_Events
```

Check that the `reason` and `source_ip` fields exist and that the event values use the exact wording shown.

## 5. Counting events by multiple fields

The learning notes describe using `stats count` with these fields:

- `ComputerName` — the machine that generated the event.
- `Target_User` — the user account targeted.
- `Caller_Process_Name` — the process associated with the login attempt.

Example:

```spl
index=main
| stats count by ComputerName Target_User Caller_Process_Name
| sort -count
```

This groups events by each unique combination of the three fields. Confirm that these fields are extracted and populated in the selected event data.

## 6. SSH authentication investigation

The following examples are based on the SSH authentication analysis performed in this portfolio project.

### 6.1 Count invalid-user attempts by username and source IP

```spl
index=main source="auth.log"
| rex "Invalid user (?<user>\\w+) from (?<ip>\\d+\\.\\d+\\.\\d+\\.\\d+)"
| stats count by user, ip
| sort -count
```

Extracts the username and source IP from matching invalid-user messages, then counts events for each username/IP pair.

### 6.2 Identify source IPs with high event volume or many targeted usernames

```spl
index=main source="auth.log"
| rex "Invalid user (?<user>\\w+) from (?<ip>\\d+\\.\\d+\\.\\d+\\.\\d+)"
| stats count dc(user) as unique_users by ip
| where count > 50 OR unique_users > 5
| sort -count
```

This highlights source IPs with more than 50 matching events or more than five distinct usernames. It is a triage query: a high count or many usernames may warrant investigation, but does not by itself prove malicious intent or successful access.

## 7. SPL commands used in these examples

| Command | Purpose |
|---|---|
| `stats` | Aggregates events into statistics |
| `count` | Counts events in each group |
| `max()` | Returns the maximum field value |
| `dc()` | Counts distinct values |
| `rex` | Extracts fields using a regular expression |
| `where` | Filters results using an expression |
| `sort -count` | Sorts count values in descending order |

## 8. Validation checklist

Before relying on a query in a SOC investigation:

1. Confirm the correct index, source, sourcetype, and time range.
2. Open representative raw events and verify the exact message format.
3. Confirm that every referenced field exists and contains the expected values.
4. Test field extractions on a small sample before aggregating.
5. Check whether the result counts events, distinct values, users, or source addresses.
6. Record limitations, missing telemetry, and any assumptions in the investigation notes.

## Source note

The Boolean-operator examples, basic search structure, port-scanning example, phishing example, and multi-field aggregation topics are based on the author's uploaded document, **Splunk Query's.pdf**. Syntax corrections and explanatory notes are explicitly identified above. SSH examples are based on searches developed during this portfolio project.
