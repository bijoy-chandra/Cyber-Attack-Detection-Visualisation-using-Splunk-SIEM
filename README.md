# Cyber Attack Detection & Visualisation using Splunk SIEM

## What is Splunk?

Splunk is a **Security Information and Event Management (SIEM)** platform that collects, indexes, searches, analyzes, and visualizes machine generated logs from various endpoints such as operating systems, web servers, firewalls, network devices, cloud platforms, and applications. Splunk operates on three main component:

- Forwarder:
The Forwarder is a lightweight Splunk agent installed on source machines (e.g., Windows, Linux, servers, or firewalls). It collects log data and securely forwards it to the Splunk Indexer for processing. It performs minimal processing to reduce system resource usage.

- Indexer:
The Indexer is the core component of Splunk that receives data from Forwarders. It parses, indexes, compresses, and stores the log data, making it searchable for analysis and investigation.

- Search Head:
The Search Head is the user interface where analysts interact with Splunk. It executes SPL (Splunk Processing Language) searches on indexed data and displays the results through dashboards, reports, and alerts.

## Overview of the project

In this project i setup a DVWA server in kali linux machine and another kali linux machine for attacking purposes. My splunk server hosted on windows 11. Here is a diagram of my project:

<img width="756" height="559" alt="image" src="https://github.com/user-attachments/assets/eb8640ae-865b-4fcf-8210-17921da617e9" />


## Objectives

1. [Deploying Splunk Enterprise Server on Windows 11](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/blob/main/Deploying%20Splunk%20Enterprise%20in%20Windows%2011.md): This section provides a step-by-step guide for setting up Splunk Server on windows 11. It guides you to from downloading to complete setup of Splunk Server.
2. [Installing Splunk Forwarder on Windows](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/blob/main/Installing%20Splunk%20Forwarder%20on%20Windows.md): This section provides a step-by-step guide for setting up Splunk Forwarder on windows. It includes Downloading Splunk forwarder for windows, installing and configuring Splunk Forwarder to collect windows event logs.
3. [Installing & Configuring Splunk Forwarder on Linux](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/blob/main/Installing%20%26%20Configuring%20Splunk%20Forwarder%20on%20Linux.md): This section provides a step-by-step guide for setting up Splunk Forwarder on linux. It includes Downloading Splunk forwarder for linux, installing and configuring Splunk Forwarder to collect linux syslog.
4. [Setting up DVWA on Apache Server](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/blob/main/Setting%20up%20DVWA%20on%20Apache%20Server.md): This section provides a step-by-step guide for setting up DVWA on Apache server. It includes installing DVWA, configuring DVWA, configuring Maria DB and Apache server.
5. [Analyzing Tunnel Logs Using Splunk SIEM](https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/project%235-analyzing-tunnel-logs-using-splunk-siem.md): This project demonstrates how to analyze tunnel log traffic (e.g., GRE, IPv4, IPv6) from Zeek IDS using Splunk SIEM. It covers uploading sample log files, performing analysis, detecting anomalies, and correlating tunnel logs with other logs for enhanced threat detection.
6. [Analyzing SMTP Logs Using Splunk SIEM](https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/project%236-analyzing-smtp-logs-using-splunk-siem.md): This project provides a structured approach for analyzing SMTP (Simple Mail Transfer Protocol) log files using Splunk SIEM. It includes steps for uploading sample log files, extracting fields, analyzing email traffic patterns, detecting anomalies, and monitoring SMTP activity.
7. [Analyzing DHCP Logs Using Splunk SIEM](https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/project%237-analyzing-dhcp-logs-using-splunk-siem.md): This project offers guidance on analyzing DHCP (Dynamic Host Configuration Protocol) log files using Splunk SIEM. It covers uploading sample log files, extracting fields, analyzing IP address assignments, detecting anomalies, and monitoring DHCP traffic.
