## DoS and DDoS attack Detection and Visualization

### Introduction
Denial of Service (DoS) & Distributed Denial of Service (DDoS) Attack is one of the most common attacks performed in cyber world with the objective of overloading the target server or application with large traffic so as to render it unavailable for any legitimate user. Potential DoS/DDoS attacks can be detected through network and firewall logs analysis using Splunk SIEM by looking for irregular traffic volume, connection attempts, and unusual activities from source IP addresses.

### Overview
In this project, we will simulate the DoS and DDoS attacks from the attacker Kali Linux machine and use Splunk SIEM to analyze the logs generated, detect any unusual traffic patterns, and visualize the attacks using the dashboards provided by Splunk SIEM.

### Prerequisites
Before starting the analysis, ensure the following:
- Splunk instance is installed and configured.
- Create a DoS or DDoS attack log 
- A kali linux attacking machine to perform DoS or DDoS attack

### Performing DoS and DDoS attack

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



## Steps to detect DoS and DDoS attack in Splunk SIEM


### 1. Top attacker IP address
- Open Splunk interface and navigate to the search bar.
- Enter the following search query to retrieve ddos event:
- Identify key fields in ddos logs such as timestamps, source IP addresses, packets.
- Use Splunk's field extraction capabilities or regular expressions to extract these fields for better analysis.
- Example extraction command:
```
index=* source="/var/log/ddos.log" 
| bucket _time span=10s
| stats count as packets by SRC
| where packets >= 100
| sort -packets
```


### 2. Identify DDoS attack from random sources
- Identify attcks from random sources in a very short time
- Regex can be difference based on the log types
```
index=* source="/var/log/ddos.log"
| bucket _time span=10s
| stats count as packet_count
        dc(SRC) as unique_sources
        values(PROTO) as protocol
        values(DPT) as dest_port
        by DST _time
| where packet_count>=1000
| sort -packet_count
```


### 3. Identify Dos and DDoS attack over time
- Detect sudden spike in your network from single source
- This is effective for identifying DoS and DDoS attack, but attack from random sources will not trigger this graph
```
index=* source="/var/log/ddos.log"
| eventstats count as packet_count by SRC
| where packet_count >= 100
| timechart count by SRC
```


### 4. Detecting DDoS attack from random sources
- One of the most important indicator for identifying DDoS attack
- Identifies which destination were affected
```
index=* source="/var/log/ddos.log"
| bucket _time span=10s
| stats count as packet_count
        dc(SRC) as unique_sources
        values(PROTO) as protocol
        values(DPT) as dest_port
        by DST _time
| where packet_count>=1000
| sort -packet_count
```



### 5. DDoS indicator over packets from different sources

```bash
index=* source="/var/log/ddos.log"
| stats count as packets dc(SRC) as unique_sources by DST
| sort -packets
```


### 6. Top affected ports
```bash
index=* source="/var/log/ddos.log"
| stats count by DPT
```

### 7. Top affected protocols
```bash
index=* source="/var/log/ddos.log"
| stats count by PROTO
```

### 8. Top attacker geolocation
```bash
index=* source="/var/log/ddos.log" 
| iplocation SRC
| stats count by Country
| sort -count
| geom geo_countries allFeatures=True featureIdField=Country
```

### Full Dashboard view

<img width="1625" height="871" alt="image" src="https://github.com/user-attachments/assets/6b6678da-7cc7-4128-ab7f-17c4553b1754" />
<img width="1619" height="441" alt="image" src="https://github.com/user-attachments/assets/43409293-8306-43ec-89c0-0f5cc3c561b4" />


