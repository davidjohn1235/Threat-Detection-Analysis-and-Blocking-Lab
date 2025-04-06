# Threat-Detection-Analysis-and-Blocking-Lab

## Objective

The Detection Lab project aimed to establish a controlled environment for simulating and detecting cyber attacks. The primary focus was to ingest and analyze logs within an EDR (Endpoint Detection and Response) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.

### Skills Learned

- Advanced understanding of SIEM/EDR concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.
- Hunt for threats using advanced EDR and telemetry tools
- Simulate attacks and understand attacker methodologies

### Tools Used

- EDR system for log ingestion, analysis and prevention.
- Sliver Attacker Framework to Generate C2 Session.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

1) Launch Windows VM and weaken it by disabling Windows Defender
   
![ZBThhvsJJS](https://github.com/user-attachments/assets/f7c050a9-7291-4f33-942d-3b469da6fb9a)

2) Install LimaCharlie (EDR) Sensor on Windows VM
   
![image](https://github.com/user-attachments/assets/92b7cd66-c0b5-4bc2-8232-6bf71e89fca1)

3) Check if Sensor is reporting in LimaCharlie Dashboard 

![image](https://github.com/user-attachments/assets/712a23c4-735d-4091-a1f1-68792085809e)

4) Configure LimaCharlie to ingest Sysmon logs from Windows VM

![image](https://github.com/user-attachments/assets/01218534-d676-483c-bc40-f53776e5166a)

5) Launch Linux VM and install Sliver C2 Framework. Launch Sliver client 

6) Start http listener and generate new http implant 

![image](https://github.com/user-attachments/assets/13d7815b-d6e4-4d0c-a8ff-346aa158d2b8)

7) Open http:// Linux VM IP:8080 and download the implant

![image](https://github.com/user-attachments/assets/e9563c29-f06b-4a1a-ae30-77ba2aa2a43e)

8) Post running the implant we can see the C2 Session commence in Sliver and we can run a few basic commands

![image](https://github.com/user-attachments/assets/185e366d-87c6-40c5-8551-7077e4776a6e)

9) From the EDR we can see the C2 Implant Proccess and see that it is suspicious

![image](https://github.com/user-attachments/assets/7762eb1f-e624-4bb5-a109-aa77e66b7a25)

10) Upon scanning with VirusTotal from within the EDR we get no matches as the Malware is new in nature

![image](https://github.com/user-attachments/assets/937737dd-ff51-4f5a-b38f-c97e654622f9)
![image](https://github.com/user-attachments/assets/d07cacec-d9b4-4b44-8004-0f5669c62883)

11) Next we generate and attack by running execute rundll32.exe C:\\windows\\System32\\comsvcs.dll, MiniDump [LSASS PID] C:\\Windows\\Temp\\lsass.dmp full.
    Upon looking for Sensitive_Process_Access in our EDR Timeline we see events related to the attack

![image](https://github.com/user-attachments/assets/d6f3f5e7-a552-40cd-a902-5c40541d2bc4)

12) From this event we create a detection rule 

Detect - 


![image](https://github.com/user-attachments/assets/0e3a5405-a7f6-4f63-98bf-fed6e0d6d5f1)

Respond - 


![image](https://github.com/user-attachments/assets/96da3917-af50-488a-a5a2-d50297d3208f)

13) We generate the attack again and this time we see the Rule creating an alert -

![image](https://github.com/user-attachments/assets/6c4cdb14-e063-4ff9-8f93-49fbff8aedb7)

14) Next we try to block an attack, for this we attack using the "vssadmin delete shadow /all"; 
    Volume Shadow Copies provide a convenient way to restore individual files or even an entire file system to a previous state which makes it a very attractive option for recovering 
    from a ransomware attack. For this reason, it’s become very predictable that one of the first signs of an impending ransomware attack is the deletion of volume shadow copies.

15) First we run this command from a "shell" created in sliver. Post this in the Detection tab we see an event for Shadow Copy Deletion

![image](https://github.com/user-attachments/assets/4ccda08d-de4c-4440-a361-1317bd38706a)

16) Based on this detection we create a rule adding a below to the respond section, we name this rule vss_deletion_kill

![image](https://github.com/user-attachments/assets/c96105c8-2279-42a6-97e8-7cf00ab93d01)

17) Post this we run the "vssadmin delete shadow /all" command again. Post running the command we try to run other commands on the shell and we see that it fails as the parent process was terminated

![image](https://github.com/user-attachments/assets/13242f2e-0fcc-4b89-b681-4db883b89909)

18) We can see from the detection section that our rule worked

![image](https://github.com/user-attachments/assets/a6b908b0-829d-44d5-bb9b-91b3f55cdb6e)




























