# SSH Monitoring Dashboard – Screenshot Gallery

This gallery documents the four visualisations created in Splunk Enterprise for the SSH monitoring use case. The screenshots are portfolio examples from the analysed dataset.

## 1. Top Attacking Source Addresses

**File:** `top-attacking-ips-anonymized.png`

Ranks source addresses by observed event count. The IP labels have been replaced with generic identifiers (Source 01–10); chart bars and counts are retained.

## 2. Attack Type Distribution

**File:** `attack-type-distribution.png`

Shows the distribution between the categories labelled Brute Force and User Enumeration in the original dashboard. These are analytical classifications based on the dashboard's SPL logic, not independently confirmed attacker intent.

## 3. Attack Activity Over Time

**File:** `attack-activity-over-time.png`

Shows event volume over the displayed period (8–13 March 2026). Peaks indicate periods of increased logged activity and should be interpreted in the context of the selected time range and available telemetry.

## 4. Most Targeted Usernames

**File:** `most-targeted-usernames.png`

Ranks usernames appearing in the authentication events. Usernames remain visible in this portfolio version, as requested by the project author. Confirm that their publication is authorised before making the screenshots public.

## Privacy and interpretation

- The source-address labels in the Top Attacking Source Addresses image are anonymised.
- The other screenshots do not display source IP addresses.
- The usernames visible in the Most Targeted Usernames chart have not been anonymised.
- Counts are event counts from the analysed dataset; they should not be interpreted as unique attacker actions or proof of compromise.
