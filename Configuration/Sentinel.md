Phase 1 : Enviroment Provisioning
1. Created a new resource group to isolate the resource
<img width="1887" height="731" alt="image" src="https://github.com/user-attachments/assets/73f7caf8-df00-4429-bdff-349246ef238e" />
2. Deployed a Log Analytics Workspace (LAW) to act as the central repository for security logs.
<img width="1698" height="522" alt="image" src="https://github.com/user-attachments/assets/996ec712-0870-4e3a-8f7d-f9e9984a5a3f" />
3. Enabling sentinel on the LAW to begin security capabilities.
   <img width="442" height="634" alt="image" src="https://github.com/user-attachments/assets/973e82d5-7528-4ef0-bb16-8ac1f8912f4c" /> 
4. Integrating Defender XDr and Sentinel
Validation: Confirmed the connector status is "Connected," ensuring a bi-directional sync of alerts and incidents.
<img width="1868" height="543" alt="image" src="https://github.com/user-attachments/assets/80f212c7-4c65-4aa2-bd4c-6519eb1d603d" />
Unified Experience: Verified that the Sentinel Blade is now active within the Microsoft Defender portal. This allows for seamless security operations, including Hunting, Workbooks, and Incidents, without switching between portals.
<img width="1849" height="676" alt="image" src="https://github.com/user-attachments/assets/8918e3c3-5300-47ca-9dd1-239be0da953e" />




