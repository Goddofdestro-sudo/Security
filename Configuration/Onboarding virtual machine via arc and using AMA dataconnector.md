Objectives: Onboard non-azure VM and ingest logs via AMA
Requirments:
1. Non azure hosted VM
2. AMA dataconnector
3. Read/write permission on the Sentinel workspace

Phase 1: On boarding VM via Arc
Navigated to azure arc and under infrastructure blade select 'Machine" and select "Onboard"
<img width="651" height="397" alt="image" src="https://github.com/user-attachments/assets/b7c17611-adb8-4c2c-b455-ddf945a92a4c" />
Download the onboarding script and run the script on the server.
<img width="860" height="444" alt="image" src="https://github.com/user-attachments/assets/1372a748-8fd8-4fb0-b3e7-b97fa8523c67" />
<img width="895" height="519" alt="image" src="https://github.com/user-attachments/assets/81f101fb-80c4-437d-9ca8-bbc44da307e6" />
Once the onboarding process has been completed, the machine is visible under Azure arc.
<img width="1859" height="282" alt="image" src="https://github.com/user-attachments/assets/84ba849e-c1a5-470b-9e76-a85509e5c536" />

Phase 2: Ingest logs from the VM to LAW.
Firstly, confirm if the azure monitor agent has been installed or not and if not,
Navigate to azure arc and VM. Then under extension add the azure monitor agent.
<img width="377" height="359" alt="image" src="https://github.com/user-attachments/assets/95d46783-1741-4730-9272-f656a091eaf2" />
<img width="649" height="280" alt="image" src="https://github.com/user-attachments/assets/b1984bdf-fea8-4478-9172-961ada2eb03a" />
<img width="1457" height="98" alt="image" src="https://github.com/user-attachments/assets/b468e368-c5cd-44d1-9b5e-15f414fc06fb" />

