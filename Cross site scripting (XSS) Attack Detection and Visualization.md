## Cross site scripting (XSS) Attack Detection and Visualization

### Introduction
Cross-Site Scripting (XSS) is one of the most prevalent methods used to attack web applications, whereby a hacker injects a harmful script into the input fields and URL of the website, thereby running malicious code within the victim's browser. The detection of possible XSS exploits on the web servers' logs can be achieved by using Splunk SIEM through analysis of the payload, suspicious requests, and harmful input.
### Overview
In this project, we will generate XSS attack from our attacking kali Linux machine and perform analytical skills to identify and visualize XSS attack within the web application.
### Prerequisites
Before starting the analysis, ensure the following:
- Splunk instance is installed and configured.
- Configure Apache server and DVWA 
- A kali linux attacking machine to perform XSS attack

### Performing Cross site scripting (XSS) attack

- Perform XSS attack from burp suite

<img width="797" height="401" alt="image" src="https://github.com/user-attachments/assets/93a1bfe2-40e5-4c7c-b8e8-30f0f583425d" />

<img width="1473" height="801" alt="image" src="https://github.com/user-attachments/assets/fa3ddaae-75a7-47df-8fae-cb5ce47fb62b" />


- We can view web application logs from **/apache2/access.log** via this command from victim machine
```bash
tail -f /var/log/apache2/access.log
```

##### Command output
<img width="1699" height="463" alt="image" src="https://github.com/user-attachments/assets/731c1a07-cade-403d-9493-b2ee617637e4" />


## Steps to detect XSS attack in Splunk SIEM


### 1. Table view of XSS attacks
- Open Splunk interface and navigate to the search bar.
- Enter the following search query to retrieve XSS event:
- Identify key fields in XSS logs such as timestamps, source IP addresses, status code, response size, payload, etc.
- Use Splunk's field extraction capabilities or regular expressions to extract these fields for better analysis.
- Example extraction command:
```
index=* source="/var/log/apache2/access.log"
| rex field=_raw "^(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| rex field=_raw "\"(?<method>\S+)\s(?<uri>\S+)\sHTTP/[^\"]+\"\s(?<status>\d{3})\s(?<size>\d+)"
| rex field=uri "(?<path>[^=]+)\?(?<query>.*)"
| where match(uri,"(?i)%3Cscript%3E|<script>|javascript:|onerror=|onload=|alert\(|document\.cookie|prompt\(|confirm\(")
| eval payload=urldecode(uri)
| eval payload=replace(payload,"&user_token=.*","")
| table _time src_ip method status size payload
```

- Table view

<img width="1469" height="748" alt="image" src="https://github.com/user-attachments/assets/c99f7431-585f-42b7-a823-9619bcd90c13" />


### 3. Top attacker IP addresses table and dashboard view
- Identify top attacker IP addresses
- Regex can be difference based on the log types
```
index=* source="/var/log/apache2/access.log"
| rex field=_raw "^(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| rex field=_raw "\"(?<method>\S+)\s(?<uri>\S+)\sHTTP/[^\"]+\"\s(?<status>\d{3})\s(?<size>\d+)"
| rex field=uri "(?<path>[^=]+)\?(?<query>.*)"
| where match(uri,"(?i)%3Cscript%3E|<script>|javascript:|onerror=|onload=|alert\(|document\.cookie|prompt\(|confirm\(")
| eval payload=urldecode(uri)
| eval payload=replace(payload,"&user_token=.*","")
| stats count by src_ip
| sort -count
```

- Table view:

<img width="1513" height="186" alt="image" src="https://github.com/user-attachments/assets/6e7bd552-db98-4127-a8e9-70048742d311" />

- Dashboard view:

<img width="723" height="426" alt="image" src="https://github.com/user-attachments/assets/9f7fe05b-c1f5-4de3-bcce-ee36e82e2d50" />


### 4. Identify XSS attack over time
- **span=5m**; It will detect multiple login attempt within 5 minutes
```
index=* source="/var/log/apache2/access.log" "*"
| rex field=_raw "^(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| rex field=_raw "\"(?<method>\S+)\s(?<uri>\S+)\sHTTP/[^\"]+\"\s(?<status>\d{3})\s(?<size>\d+)"
| rex field=uri "(?<path>[^=]+)\?(?<query>.*)"
| where match(uri,"(?i)%3Cscript%3E|<script>|javascript:|onerror=|onload=|alert\(|document\.cookie|prompt\(|confirm\(")
| eval payload=urldecode(uri)
| eval payload=replace(payload,"&user_token=.*","")
| timechart span=5m count
```

- Dashboard view

<img width="736" height="430" alt="image" src="https://github.com/user-attachments/assets/ac79ebae-5727-4c7d-b573-393fe1d7c758" />



### 5. HTTP response code over XSS attempts
- One of the most important indicator for identifying successful XSS attack
```
index=* source="/var/log/apache2/access.log" "*"
| rex field=_raw "^(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| rex field=_raw "\"(?<method>\S+)\s(?<uri>\S+)\sHTTP/[^\"]+\"\s(?<status>\d{3})\s(?<size>\d+)"
| rex field=uri "(?<path>[^=]+)\?(?<query>.*)"
| where match(uri,"(?i)%3Cscript%3E|<script>|javascript:|onerror=|onload=|alert\(|document\.cookie|prompt\(|confirm\(")
| eval payload=urldecode(uri)
| eval payload=replace(payload,"&user_token=.*","")
| stats count by status
```

- Dashboard view

<img width="724" height="432" alt="image" src="https://github.com/user-attachments/assets/22132bfe-611c-4dcb-85cb-8a5760580279" />


### 5. Top targeted path 

```bash
index=* source="/var/log/apache2/access.log" "*"
| rex field=_raw "^(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| rex field=_raw "\"(?<method>\S+)\s(?<uri>\S+)\sHTTP/[^\"]+\"\s(?<status>\d{3})\s(?<size>\d+)"
| rex field=uri "(?<path>[^=]+)\?(?<query>.*)"
| where match(uri,"(?i)%3Cscript%3E|<script>|javascript:|onerror=|onload=|alert\(|document\.cookie|prompt\(|confirm\(")
| eval payload=urldecode(uri)
| eval payload=replace(payload,"&user_token=.*","")
| stats count by path
| sort -count
```

- Table view

<img width="733" height="428" alt="image" src="https://github.com/user-attachments/assets/30a28722-0e71-4f22-8907-542bb8394a7e" />


