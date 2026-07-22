## Cross site scripting (XSS) Attack Detection and Visualization

### Introduction
Cross-Site Scripting (XSS) is one of the most prevalent methods used to attack web applications, whereby a hacker injects a harmful script into the input fields and URL of the website, thereby running malicious code within the victim's browser. The detection of possible XSS exploits on the web servers' logs can be achieved by using Splunk SIEM through analysis of the payload, suspicious requests, and harmful input.

### Overview
In this project, we will generate XSS attack from our attacking kali Linux machine and perform analytical skills to identify and visualize XSS attack within the web application.

### Prerequisites
Before starting the analysis, ensure the following:
- Splunk instance is installed and configured.
- Create a DoS or DDoS attack log 
- A kali linux attacking machine to perform DoS or DDoS attack

### Performing Cross site scripting (XSS) attack

- Perform these following commands on victim machine
- Create an iptables rule to log all incoming TCP SYN packets on port 80 with the prefix "TCP-DDOS:"
- **journalctl** : Monitors kernel logs, filters "TCP-DDOS" entries, and saves them to /var/log/ddos.log for Splunk analysis.
```bash
sudo iptables -A INPUT -p tcp --dport 80 --syn -j LOG --log-prefix "TCP-DDOS: " --log-level 4
sudo su
journalctl -k -f | grep "TCP-DDOS" >> /var/log/ddos.log
```

- Now Generate an simple DDoS attack using random source
```bash
sudo hping3 -S -p 80 --flood --rand-source 192.168.132.130
```
- Generate an DoS attack
```bash
sudo hping3 -S -p 80 --flood 192.168.132.130
```

<img width="831" height="367" alt="image" src="https://github.com/user-attachments/assets/59419904-2d92-4690-b746-9b6afe763f74" />


- We can view DDoS attack logs from **var/log/ddos.log** via this command from victim machine
```bash
tail -f /var/log/ddos.log
```

##### Command output
<img width="1705" height="436" alt="image" src="https://github.com/user-attachments/assets/8bad8eed-6ac3-44a6-b8d6-40766aa4417e" />



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
