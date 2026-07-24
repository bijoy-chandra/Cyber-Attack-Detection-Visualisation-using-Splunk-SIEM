# Cyber Attack Detection & Visualisation using Splunk SIEM

##

Today, modern businesses produce huge amounts of security logs on servers, endpoints, network devices, and web applications. Manual analysis of such logs is both time consuming and ineffective, which makes it challenging to detect any malicious activity instantly. Splunk is a popular SIEM solution that helps security analysts to collect, search, analyze and visualize machine-generated security logs for various sources to detect cyber attacks, conduct investigations, generate automated alerts, and create dashboards that provide real-time visualization of security status.

This project shows how Splunk Enterprise can be utilized to collect logs from Windows and Linux operating systems, detect multiple cyber attacks within a lab setting, and visualize attack patterns using interactive dashboards.

### Splunk Architecture

Log data analysis in Splunk involves three main parts:

1. Universal Forwarder:
The Splunk Universal Forwarder is a light weight software agent which runs on Windows or Linux or any other source system. It collects the log data continuously and sends it to the Splunk Indexer.

2. Indexer:
Splunk Indexer received data by one or more forwarders is parsed and indexed in the indexer. Data compression and storage make the searching of data more efficient. The indexer is the key component of the Splunk system because it makes the huge amount of security logs searchable.

3. Search Head:
The Search Head is the interface provided by Splunk in the form of web-based application, where security analysts interact with Splunk. It performs search operations using Splunk Processing Language (SPL). The Search Head queries the indexed data and displays the output using dashboards, reports, visualizations, and alerts.

## Overview of the project

This lab explains the process of using Splunk SIEM in detecting, investigating, and visualizing different cyber attacks in a safe lab setting.

The lab setup includes the following components:

- A Kali Linux system running the Damn Vulnerable Web Application (DVWA) application as the targeted system.
- Another Kali Linux system, which is responsible for launching simulated cyber attacks on the target system.
- A Windows 11 machine hosting the Splunk Enterprise system.

All the logs received from the target system are forwarded to the Splunk system, where they undergo parsing and indexing by means of SPL queries and dashboard creation.

<img width="756" height="559" alt="image" src="https://github.com/user-attachments/assets/eb8640ae-865b-4fcf-8210-17921da617e9" />


## Objectives

1. [Deploying Splunk Enterprise Server on Windows 11](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/blob/main/Deploying%20Splunk%20Enterprise%20in%20Windows%2011.md): This section explains how to install and configure Splunk Enterprise on Windows 11. The steps include downloading the software installer, installation, configuration of the software, and confirmation that the Splunk server works.
2. [Installing Splunk Forwarder on Windows](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/blob/main/Installing%20Splunk%20Forwarder%20on%20Windows.md): This section will explain the process of installing Splunk Universal Forwarder in Windows. The process involves downloading the Forwarder, its installation, configuration of inputs and outputs, and forward Windows Event Logs to the Splunk Enterprise Server.
3. [Installing & Configuring Splunk Forwarder on Linux](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/blob/main/Installing%20%26%20Configuring%20Splunk%20Forwarder%20on%20Linux.md): This section demonstrates how to install and configure the Splunk Universal Forwarder on Linux systems. It includes configuring log monitoring, forwarding Linux system logs (Syslog, authentication logs, etc.), and validating successful communication with the Splunk server.
4. [Setting up DVWA on Apache Server](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/blob/main/Setting%20up%20DVWA%20on%20Apache%20Server.md): This document will guide you through the process of setting up Damn Vulnerable Web Application (DVWA) on an Apache web server operating on Kali Linux. This includes:
- Setting up the database 
- Installation of DVWA
- Setting up Apache and MariaDB(mysql) 
- Testing the installation

DVWA is set up to act as the vulnerable application in this project.

5. [SSH Brute Force Attack Detection and Visualization](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/blob/main/SSH%20Brute%20Force%20Attack%20Detection%20and%20Visualization.md): In this project, we illustrate the capability of Splunk SIEM in detecting and visualizing SSH brute-force attacks using the Linux authentication logs.

The above analysis involves the following steps:

- Collecting the SSH authentication logs
- Detecting multiple failed login attempts
- Identifying successful logins after brute-force attacks
- Identifying the source IPs of the attackers
- Analyzing the frequency of attacks with time
- Visualization of brute-force attacks using dashboards

6. [Cross site scripting (XSS) Attack Detection and Visualization](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/blob/main/Cross%20site%20scripting%20(XSS)%20Attack%20Detection%20and%20Visualization.md): This project provides an insight into how Splunk SIEM is able to detect and analyze XSS attacks through the analysis of the Apache web server access logs for DVWA.

Some of the steps involved in the process include:

- Gathering the Apache access logs
- Identifying any malicious XSS payloads in HTTP requests
- Identifying the IP address of the attackers and the target URLs
- Analyzing request patterns
- HTTP response code monitoring
- Creating dashboards to visualize the frequency of attacks and the attacked web resources

7. [DoS and DDoS attack Detection and Visualization](https://github.com/bijoy-chandra/Cyber-Attack-Detection-Visualisation-using-Splunk-SIEM/edit/main/DoS%20and%20DDoS%20attack%20Detection%20and%20Visualization.md): In this project, it is shown how Splunk SIEM could be used to discover and analyze DoS/DDoS attacks based on the network and systems' logs.

The discovery process involves:

- Collection of attack traffic logs
- Discovery of abnormally high spikes in the network traffic
- Discovery of high requests volume
- Analysis of source IP addresses that cause the attack
- Tracking attacked services and destination ports
