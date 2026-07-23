**Scheduled Analytics Rule with Entity Mapping**
Scenario simulated: Impossible Travel (Sign-in Anomaly) MITRE ATT&CK Mapping: T1078 – Valid Accounts Data Source: Microsoft Entra ID Data Connector 

**1. Objective**
Detect a single user authenticating from two geographically distant locations within a timeframe too short for real travel, using a custom Sentinel Scheduled Analytics Rule — including entity mapping and incident grouping configuration.
This lab focuses on detection engineering fundamentals: writing the detection logic from scratch rather than relying on a built-in template, to understand what native Identity Protection risk detections are actually doing under the hood.

**2. Test Setup**
Created a dedicated test account (Global Reader role) to avoid triggering real Conditional Access / Identity Protection controls on a production identity.
<img width="847" height="393" alt="image" src="https://github.com/user-attachments/assets/57bf5d56-8e47-444c-a5df-0075dcaea1af" />

Baseline sign-in performed with no VPN (Australia-based IP).
<img width="2429" height="1064" alt="image" src="https://github.com/user-attachments/assets/23502bba-8b71-43a5-94b7-cebe9ae95a63" />

Second sign-in performed shortly after, connected via VPN with a US exit node, in a separate browser session to avoid silent session/token reuse.
<img width="2560" height="1392" alt="image" src="https://github.com/user-attachments/assets/653f6a81-ed72-40d7-b947-3d9e51d63c22" />

Confirmed both sign-in events landed in SigninLogs (allowing ~10–15 minutes for ingestion delay from Entra ID → Log Analytics).

<img width="1428" height="697" alt="image" src="https://github.com/user-attachments/assets/ef8799ee-2d12-4cfc-b121-a8a70776387e" />


**3. Detection Query**

Initial validation query (raw event check):

kql
SigninLogs
| where TimeGenerated > ago(2h)
| where ResultType == 0
| project TimeGenerated, UserPrincipalName, IPAddress, Location, AppDisplayName
| order by TimeGenerated asc

Final detection logic (grouped, threshold-based):

kql
let timeWindow = 2h;
SigninLogs
| where TimeGenerated > ago(timeWindow)
| where ResultType == 0
| project TimeGenerated, UserPrincipalName, IPAddress, Location, AppDisplayName
| summarize
    LocationsSeen = make_set(Location),
    IPsSeen = make_set(IPAddress),
    SignInCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by UserPrincipalName
| where array_length(LocationsSeen) > 1
| extend TimeGapMinutes = datetime_diff('minute', LastSeen, FirstSeen)
| where TimeGapMinutes < 60
| extend LatestIP = tostring(IPsSeen[array_length(IPsSeen)-1])

Logic summary: groups sign-ins per user over a rolling window, flags users seen from more than one distinct location, and filters to cases where the time gap between first and last sign-in is too short to be realistic travel.

4. Entity Mapping
Entity	Identifier	Field Mapped
Account	Name	UserPrincipalName
IP	Address	LatestIP

<img width="1457" height="1212" alt="image" src="https://github.com/user-attachments/assets/a321ef48-ff6c-45b8-9478-6a4b860609e3" />

Key lesson: entity mapping requires a single scalar value per row, not an array. The initial query output IPsSeen (from make_set()) failed to map cleanly to the IP entity because it was a dynamic array. Adding LatestIP = tostring(IPsSeen[array_length(IPsSeen)-1]) extracted a single representative value that the entity mapping engine could bind to.

What entity mapping does vs. doesn't do:

It has no effect on whether the rule fires — that's determined entirely by the KQL query logic.
It governs what happens after an alert triggers: populating structured entity cards on the incident, enabling entity-based correlation across other rules/incidents (and UEBA), and allowing automation playbooks to reference {{Entity.Account.Name}} / {{Entity.IP.Address}} directly.

5. Incident Settings
Grouping: enabled, grouped by Account entity match only (not IP) — since the IP is expected to change between the AU and US sign-ins, grouping on IP would have split what should be a single ongoing incident for the same user into separate incidents.
Reopen closed matching incidents: enabled — prevents a resolved incident from masking a genuine recurrence.
Grouping time window: 24 hours.
Automated response: left empty for this lab — a Logic Apps playbook (e.g. auto-disable account, notify SOC) is a planned follow-up once entity mapping and detection logic were validated first.
<img width="1325" height="782" alt="image" src="https://github.com/user-attachments/assets/043c043c-0bad-4012-86e2-cb6012967bff" />
<img width="1388" height="334" alt="image" src="https://github.com/user-attachments/assets/a8ff385e-e207-46df-bbc7-09d5ec900817" />

6. Result

Rule triggered successfully on the next scheduled run. Incident formed in the Sentinel Incidents queue with:

Account entity populated (test account UPN)
IP entity populated (US VPN exit IP)
Both AU and US sign-in events correlated under the same user
<img width="2022" height="105" alt="image" src="https://github.com/user-attachments/assets/cc336ca8-79fe-416c-b775-6d7f75b21d03" />
<img width="1038" height="345" alt="image" src="https://github.com/user-attachments/assets/6cb00af8-1dfe-41ce-973f-f32a943a529d" />

7. Key Takeaways
Built the detection query, entity mapping, and incident grouping logic manually to understand what native Entra ID Identity Protection ("Impossible travel" / "Atypical travel" risk detections) does automatically under the hood via machine learning — in a production tenant with P2 licensing, the native risk detection would typically be used instead of a custom rule like this.
Custom rules are still valuable for scenarios native detection doesn't cover (e.g. org-specific sensitive app access from unexpected countries).
Array vs. scalar field handling is a common, easy-to-miss blocker in entity mapping — worth checking query output shape before configuring entities.
Grouping incidents by the right entity (Account, not IP) matters when the anomalous signal is expected to vary between alerts.
