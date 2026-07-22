## SSH Brute Force Attack Detection and Visualization

### Introduction
SSH (Secure Shell) authentication logs contain useful data regarding the login attempts, which include successful and unsuccessful authentication events. Security analysts use Splunk SIEM to analyze these logs and detect SSH brute force attacks by identifying multiple login failures. This will allow for the monitoring of activities carried out by the attackers in real time, as well as visualization of the attacks through interactive dashboards.

### Overview
In this project, we will generate SSH brute force attack from our attacking kali Linux machine and perform analytical skills to identify and visualize SSH brute force attack within the network.

### Prerequisites
Before starting the analysis, ensure the following:
- Splunk instance is installed and configured.
- SSH log data sources are configured to forward logs to Splunk.
- A kali linux attacking machine to perform brute force attack

### Performing a SSH brute force attack

- Command to produce a ssh brute force attack
```bash
hydra -l kali -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ssh://192.168.132.130
```

- We can view authentication logs from **auth.log** via this command from victim machine
```bash
tail -f /var/log/auth.log
```

##### Command output
<img width="1697" height="743" alt="image" src="https://github.com/user-attachments/assets/ca06a9e6-60c8-4f0b-a14e-96a2f8082fe0" />


## Steps to detect SSH brute force attack in Splunk SIEM


### 1. Search for SSH Events
- Open Splunk interface and navigate to the search bar.
- Enter the following search query to retrieve SSH failed and successful login event:
- index=*  means searching through the every index. We can also specify exact index to look ssh authentication logs.
```
index=* ("Failed password" OR "Accepted password")
```

### 2. Table view of SSH brute force attack
- Identify key fields in SSH logs such as timestamps, source IP addresses, usernames, actions, etc.
- Use Splunk's field extraction capabilities or regular expressions to extract these fields for better analysis.
- Example extraction command:
```
index=* ("Failed password" OR "Accepted password") "*"
| table _time src host vendor_action action app
```

- Table view

<img width="1465" height="628" alt="image" src="https://github.com/user-attachments/assets/489a2998-f30d-4e63-8b47-263300b690ff" />


### 3. Top attacker IP addresses table and dashboard view
- Identify top attacker IP addresses
- Regex can be difference based on the log types
```
index=* source="/var/log/auth.log"
| rex field=src "(?:::ffff:)?(?<src_ip>\d{1,3}(?:\.\d{1,3}){3})"
| eval src=coalesce(src_ip,src)
| stats count(eval(action="failure")) as Failed
        count(eval(action="success")) as Success
        by src
| where Failed >= 5 AND Success >= 0
```

- Table view:

<img width="1515" height="149" alt="image" src="https://github.com/user-attachments/assets/fa0546fe-006a-4397-b7c1-08cb41a999ab" />

- Dashboard view:

<img width="723" height="426" alt="image" src="https://github.com/user-attachments/assets/a4121cb6-1b5a-4079-9f67-abeddaac9480" />



### 4. Identify brute force attack over time
- Key indicator is multiple login attempt in a short span of time.
- **span=5m**; It will detect multiple login attempt within 5 minutes
```
index=* ("Failed password" OR "Accepted password") "*"
| timechart span=5m count  ## Based on situation time can be increase and decrease
```

- Dashboard view

<img width="735" height="429" alt="image" src="https://github.com/user-attachments/assets/ffcb5401-fdaa-4acb-8e41-830088c9a312" />



### 5. Top attacker geolocation 
- Look for login attempt from unusual geolocation
```
index=* source="/var/log/auth.log" "*"
| stats count(eval(action="failure")) as Failed
        count(eval(action="success")) as Success
        by src
| where Failed >= 5 AND Success >= 1
| iplocation src
| stats count by Country 
| geom geo_countries allFeatures=True featureIdField=Country
```

- Dashboard view

<img width="1465" height="655" alt="image" src="https://github.com/user-attachments/assets/fcb3c22f-9de4-4a42-b9aa-05f06f3cb73f" />

