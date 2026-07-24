## SSH Brute Force Attack Detection and Visualization

### Introduction
SSH is the most widely used protocol for the remote administration of Linux/Unix based systems. Due to direct server access, SSH protocol is the prime target for malicious users trying to hack into the system. An SSH brute force attack is one of the most popular ways of performing an attack, where the attacker tries different usernames/passwords combinations until he/she finds the correct one. This kind of attacks produces many failed logins in a short period of time and finally leads to a successful login in case of weak authentication.

In this research, we use Splunk SIEM for collecting and analyzing the Linux authentication logs and detecting SSH brute force attacks. Using SPL commands we find suspicious activities, analyze the attacker's actions and create interactive dashboards to present attack trends.

### Overview
This project is a demonstration of how to generate an SSH brute force attack on a testbed and detect the attack via Splunk SIEM.

The process is comprised of:

- Brute force attack generation on SSH from an attacking Kali Linux machine
- Data collection of Linux authentication logs via Splunk Universal Forwarder
- Authentication failure detection
- Successful login detection after brute force attacks
- Detection of the busiest attacker IP addresses
- Attack visualization via Splunk dashboards

### Prerequisites
Before beginning the analysis process, the following prerequisites should be satisfied:

- Splunk Enterprise is set up.
- Splunk Universal Forwarder is configured to collect the Linux authentication logs.
- The SSH service is running on the target Linux system.
- A Kali Linux attacker machine is set up.
- Network connection is present between the attacker, victim, and Splunk server.

### Key Indicators of SSH Brute Force Attacks

The following are some of the signs that security analysts should look for when examining SSH authentication logs:

- Multiple failed login attempts from one IP address
- High rate of authentication failures within a short period of time
- Immediate successful login attempt after many failed ones
- Login attempts using several usernames from one IP address
- Connection requests from unlikely geographical locations
- Repeated authentication attempts outside office hours
- Multiple IP addresses attacking the same SSH server

### Performing a SSH brute force attack

For demonstration purposes, a brute force attack is generated using Hydra against the target SSH server.

**Hydra Command**
```bash
hydra -l kali -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ssh://192.168.132.130
```

During the attack, authentication events can be monitored on the victim machine using:
```bash
tail -f /var/log/auth.log
```

##### Sample Authentication Logs
<img width="1697" height="743" alt="image" src="https://github.com/user-attachments/assets/ca06a9e6-60c8-4f0b-a14e-96a2f8082fe0" />


## Detecting SSH Brute Force Attacks in Splunk SIEM


### 1. Search for SSH Authentication Events
- Open Splunk interface and navigate to the search bar.
- Enter the following search query to retrieve SSH failed and successful login event:
- index=* will search all indexes. However, in a production environment, you are advised to search the relevant index where your Linux authentication logs exist.
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
- The **span** value can be adjusted depending on the expected attack frequency.
```
index=* ("Failed password" OR "Accepted password") "*"
| timechart span=5m count  ## Based on situation time can be increase and decrease
```

- Dashboard view

<img width="735" height="429" alt="image" src="https://github.com/user-attachments/assets/ffcb5401-fdaa-4acb-8e41-830088c9a312" />



### 5. Analyze Attacker Geolocation
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


### Investigation Summary

SSH authentication logs analysis using Splunk SIEM helps to detect quickly the brute-force attacks and comprehend the attacker's behavior.

The analysis reveals information about:

- Source IP addresses initiating multiple login attempts
- Number of failed/unsuccessful and successful authentication attempts
- Trends in terms of time
- Country from which the attacks originate
- Attack rate

The interactive dashboards help to spot the malicious activities much faster.
