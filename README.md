# Cyber Attack Detection & Visualisation using Splunk SIEM

## What is Splunk?

Splunk is a **Security Information and Event Management (SIEM)** platform that collects, indexes, searches, analyzes, and visualizes machine generated logs from various endpoints such as operating systems, web servers, firewalls, network devices, cloud platforms, and applications. Splunk operates on three main component:

1. Forwarder
The Forwarder is a lightweight Splunk agent installed on source machines (e.g., Windows, Linux, servers, or firewalls). It collects log data and securely forwards it to the Splunk Indexer for processing. It performs minimal processing to reduce system resource usage.

2. Indexer
The Indexer is the core component of Splunk that receives data from Forwarders. It parses, indexes, compresses, and stores the log data, making it searchable for analysis and investigation.

3. Search Head
The Search Head is the user interface where analysts interact with Splunk. It executes SPL (Splunk Processing Language) searches on indexed data and displays the results through dashboards, reports, and alerts.

## Overview of the project

In this project i setup a DVWA server in kali inux machine and another kali linux machine for attacking purposes. My splunk server hosted on windows 11. Here is a diagram of my project:

<img width="756" height="559" alt="image" src="https://github.com/user-attachments/assets/eb8640ae-865b-4fcf-8210-17921da617e9" />


## Objectives

1. [Analyzing DNS Logs Using Splunk SIEM](https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/Project%231-analyzing-dns-log-using%20splunk-siem.md): This project provides a step-by-step guide for analyzing DNS (Domain Name System) log files using Splunk SIEM. It covers uploading sample log files, extracting relevant fields, analyzing DNS query patterns, detecting anomalies, and monitoring DNS traffic.
2. [Analyzing FTP Logs Using Splunk SIEM](https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/project%232-analyzing-ftp-logs-using-splunk-siem.md): This project guides you through analyzing FTP (File Transfer Protocol) log files using Splunk SIEM. It includes steps for uploading sample log files, extracting fields, analyzing FTP activity patterns, detecting anomalies, and monitoring FTP traffic.
3. [Analyzing HTTP Logs Using Splunk SIEM](https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/project%233-analyzing-http-logs-using-splunk-siem.md): This project outlines the process of analyzing HTTP (Hypertext Transfer Protocol) log files using Splunk SIEM. It covers uploading sample log files, extracting relevant fields, analyzing HTTP request patterns, detecting anomalies, and monitoring HTTP traffic.
4. [Analyzing SSH Logs Using Splunk SIEM](https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/project%234-analyzing-ssh-logs-using-splunk-siem.md): This project provides a comprehensive guide for analyzing SSH (Secure Shell) log files using Splunk SIEM. It includes steps for uploading sample log files, extracting fields, analyzing SSH activity patterns, detecting anomalies, and correlating SSH logs with other data sources.
5. [Analyzing Tunnel Logs Using Splunk SIEM](https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/project%235-analyzing-tunnel-logs-using-splunk-siem.md): This project demonstrates how to analyze tunnel log traffic (e.g., GRE, IPv4, IPv6) from Zeek IDS using Splunk SIEM. It covers uploading sample log files, performing analysis, detecting anomalies, and correlating tunnel logs with other logs for enhanced threat detection.
6. [Analyzing SMTP Logs Using Splunk SIEM](https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/project%236-analyzing-smtp-logs-using-splunk-siem.md): This project provides a structured approach for analyzing SMTP (Simple Mail Transfer Protocol) log files using Splunk SIEM. It includes steps for uploading sample log files, extracting fields, analyzing email traffic patterns, detecting anomalies, and monitoring SMTP activity.
7. [Analyzing DHCP Logs Using Splunk SIEM](https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/project%237-analyzing-dhcp-logs-using-splunk-siem.md): This project offers guidance on analyzing DHCP (Dynamic Host Configuration Protocol) log files using Splunk SIEM. It covers uploading sample log files, extracting fields, analyzing IP address assignments, detecting anomalies, and monitoring DHCP traffic.
