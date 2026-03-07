**Objectives: Onboard non-azure VM and ingest logs via AMA
Requirments:
1. Non azure hosted VM
2. AMA dataconnector
3. Read/write permission on the Sentinel workspace**

**Phase 1: On boarding VM via Arc
Navigated to azure arc and under infrastructure blade select 'Machine" and select "Onboard"**

<img width="651" height="397" alt="image" src="https://github.com/user-attachments/assets/b7c17611-adb8-4c2c-b455-ddf945a92a4c" />

**Download the onboarding script and run the script on the server.**

<img width="860" height="444" alt="image" src="https://github.com/user-attachments/assets/1372a748-8fd8-4fb0-b3e7-b97fa8523c67" />
<img width="895" height="519" alt="image" src="https://github.com/user-attachments/assets/81f101fb-80c4-437d-9ca8-bbc44da307e6" />

**Once the onboarding process has been completed, the machine is visible under Azure arc.**

<img width="1859" height="282" alt="image" src="https://github.com/user-attachments/assets/84ba849e-c1a5-470b-9e76-a85509e5c536" />

**Phase 2: Ingest logs from the VM to LAW.
Azure agent needs to be installed on the VM for it to start transfering the logs.**

**Navigate to azure arc and VM. Then under extension add the azure monitor agent.**

<img width="377" height="359" alt="image" src="https://github.com/user-attachments/assets/95d46783-1741-4730-9272-f656a091eaf2" />
<img width="649" height="280" alt="image" src="https://github.com/user-attachments/assets/b1984bdf-fea8-4478-9172-961ada2eb03a" />
<img width="1457" height="98" alt="image" src="https://github.com/user-attachments/assets/b468e368-c5cd-44d1-9b5e-15f414fc06fb" />

**To ensure your lab environment aligns with current security engineering standards, we will use the Windows Security Events via AMA connector. This represents the modern, Azure Monitor Agent (AMA)-based approach to log ingestion.

1. Enable the Connector
In the Microsoft Sentinel portal, navigate to Data Connectors and enable the Windows Security Events via AMA connector. This provides the backend necessary to receive, process, and store your Windows security data in a centralized Log Analytics Workspace.

3. Define Data Collection Rules (DCR)
Once the connector is enabled, you must create a Data Collection Rule (DCR) to filter incoming logs. Rather than ingesting the entire event stream, we will configure a Custom filter to minimize costs and reduce "noise."

Configure the DCR: Navigate to the Azure Monitor portal and create a new DCR.

Specify XPath Filtering: Under the "Collect" tab, select Custom and apply the following XPath expression to capture only successful logon events (Event ID 4624):

Security!*[System[(EventID=4624)]]

Association: Ensure the DCR is linked to your target machine via the Resources tab to initiate the log streaming process.**

<img width="599" height="831" alt="image" src="https://github.com/user-attachments/assets/39bf5a5a-417a-42c1-ab2c-b5baeb4df346" />

**Validate logs ingestion via KQL**

<img width="1118" height="715" alt="image" src="https://github.com/user-attachments/assets/ea022732-bbaf-4926-b1cd-b0f81db96c8a" />


