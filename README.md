<h1>Manual Alert Creation</h1>
<b>This tutorial outlines the configuration </b>

<h2>Environments and Technologies Used</h2>

- <b>Microsoft Azure</b> 
- <b>Description</b>
- <b>Description</b>
- <b>Description</b>
- <b>Description</b>

<h2>Operating Systems</h2>

- <b>Windows 10</b>

<h2>Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/e035c791-e657-4a6b-91ea-b44ab9afd94e)
- <b>Navigate to Microsoft Sentinel and click analytics</b>
- <b>Click create and scheduled query rule</b>

![image](https://github.com/user-attachments/assets/6606b270-8e95-4d08-8a9f-622626b1ea3a)
- <b>Name: TEST: Brute Force ATTEMPT - Windows</b>
- <b>Description: When the same person fails to log into the same VM at least 10 times in the last 60 minutes</b>
- <b>Click Next:Set rule logic</b>

![image](https://github.com/user-attachments/assets/bfc36842-9dd8-4922-ae33-59872566d605)
- <b>Paste this on the rule query:</b>
``` 
SecurityEvent
| where EventID == 4625
| where TimeGenerated > ago(60m)
| summarize FailureCount = count() by AttackerIP = IpAddress, EventID, Activity, DestinationHostName = Computer
| where FailureCount >= 10
```
- <b>Click entity mapping and add new entity</b>
- <b>Select IP</b>
- <b>Select Address: AttackerIP</b>
- <b>Select Host</b>
- <b>Select HostName: DestinationHostName</b>
- <b>Run query every: 5 minutes</b>
- <b>Click next: incident settings</b>
- <b>Enable Group related alerts, triggered by this analytics rule, into incidents</b>

![image](https://github.com/user-attachments/assets/2a01a39d-e45f-4ff8-8d0e-2e5f32a0143f)
- <b>Perform 10 failed login attempts into your windows-vm</b>

![image](https://github.com/user-attachments/assets/e7553ab4-b1fa-450b-8513-8e41c0ffd67a)
- <b>Navigate to Log Analytics Workspace</b>
- <b>Use this query to observe the failed login attempts from your windows-vm:</b>
```
SecurityEvent
| where EventID == 4625
| where TimeGenerated > ago(60m)
| summarize FailureCount = count() by AttackerIP = IpAddress, EventID, Activity, DestinationHostName = Computer
| where FailureCount >= 10
```

![image](https://github.com/user-attachments/assets/f052ba78-a37e-44f8-991f-d1582a8622fc)
- <b>Navigate to Microsoft Sentinel and click incidents</b>
