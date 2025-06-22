# Brute Force Attack Detection using Splunk

## 🛡️ Overview
This project demonstrates how to detect brute-force login attempts using **Splunk SIEM**. 
It simulates real-world failed login activity (EventCode 4625) from a Windows machine and triggers an alert when multiple failed attempts are detected in a short time frame.


## 🧰 Tools Used
- Splunk Enterprise (host machine)
- Splunk Universal Forwarder (Windows VM)
- Windows 10 VM (for log generation)
- EventCode 4625 (Failed Logon)


## ⚙️ Setup & Configuration

### 1. Installed Splunk Enterprise on Host
- Configured listening on port `9997` for incoming logs
![p0](https://github.com/user-attachments/assets/f2d0f221-1518-4c3f-a742-c5f9a905438e)

### 2. Installed Splunk Universal Forwarder in Windows VM
- Configured `outputs.conf` and `inputs.conf` to forward Security logs to Splunk
1) outputs.conf:
  [tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = 192.168.xx.xx:9997

[tcpout-server://192.168.xx.xx:9997]

2) inputs.conf:
[default]
host = WindowsVM

[WinEventLog://Security]
disabled = 0

[WinEventLog://Microsoft-Windows-PowerShell/Operational]
disabled = 0


### 3. Generated Attack Data
- Simulated multiple failed logins (Event ID 4625) using invalid credentials
  ![p1](https://github.com/user-attachments/assets/5209e67f-83d9-432b-99ce-ff7168b79e4d)


### 4. SPL Query to Detect Brute Force
index=* EventCode=4625
| stats count by Account_Name, host
| where count >= 3
![p2](https://github.com/user-attachments/assets/883e801b-ead5-4315-8969-e7dcde571ea4)

### 5. Alert Creation in Splunk
- Trigger alert if failed login attempts exceed threshold

- Scheduled alert to run every hour

- Added to Triggered Alerts list
![p3](https://github.com/user-attachments/assets/36d7776c-d4d7-49ef-9175-21a6ee164ac8)

✅ Outcome
- Successfully detected brute-force attempts

- Alert generated when login failures exceeded threshold

- Demonstrated complete SIEM workflow using Splunk

