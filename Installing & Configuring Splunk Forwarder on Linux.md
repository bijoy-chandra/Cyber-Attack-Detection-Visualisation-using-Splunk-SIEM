## Installing & Configuring Splunk Forwarder on Linux

This section provides an example of setting up the Splunk Universal Forwarder on Linux. This software collects the logs from a Linux machine and sends them to the Splunk Enterprise server where they can be centrally monitored.


### Prerequisites

Before installing, ensure that the following prerequisites are fulfilled:

- Splunk Enterprise must be installed and running.
- The receiving port (default 9997) needs to be set up on the Splunk Enterprise server.
- There should be network connectivity between the Linux box and the Splunk server.
- Access to root or sudo access is necessary.
- Internet connection must be available for downloading the installation package.


### Step 1: Verify the Linux Architecture

Before downloading the installer, find out your Linux Kernel version and system architecture, which will help you identify the appropriate Splunk Universal Forwarder installation package.

 ```bash
uname -a
uname -r
```

Here, the system architecture for this lab is Linux AMD64, therefore we will be using Debian (.deb) package.

<img width="1067" height="624" alt="image" src="https://github.com/user-attachments/assets/7725f6a1-4c3b-49d1-b738-5e39c1ca24bd" />

### Step 2: Download the Splunk Universal Forwarder

Open Splunk's [Downloads page](https://www.splunk.com/en_us/blog/learn/splunk-universal-forwarder.html) and choose the right package according to your Linux operating system.

For Debian-based operating systems:

- Go to Linux
- Choose AMD64
- Pick the .deb package
- Get the wget download link

Run the wget command in the Linux terminal.

<img width="1196" height="852" alt="image" src="https://github.com/user-attachments/assets/4c503bab-67fa-45cb-9ffb-c32f9dfc07aa" />

### Step 3: Install the Universal Forwarder

Update the package repository and install the downloaded package.

```bash
sudo apt update
sudo apt install ./splunkforwarder-10.4.1-5a009d941268-linux-amd64.deb
```

Once the installation process is finished, launch the Splunk Universal Forwarder and confirm the license acceptance.

```bash
/opt/splunkforwarder/bin/splunk start --accept-license --answer-yes
```

On the first run of the program, you will need to create:

- Username for Administrator
- Password for Administrator

Then the Universal Forwarder service will start.

<img width="976" height="685" alt="image" src="https://github.com/user-attachments/assets/6e8d0154-d343-4a0e-adef-fdd7bddc6a10" />


### Step 4: Verify User Permissions

Universal Forwarder operates with the **splunkfwd** service account.

For monitoring of system log files from the **/var/log** directory, the account should be part of the **adm** group.

Check the group membership of the account:

```bash
getent group adm
```

If **splunkfwd** is not there, include it in the adm group:

```bash
sudo usermod -aG adm splunkfwd
```

This enables the Universal Forwarder to access the restricted log files like **auth.log, syslog, kern.log, daemon.log**, etc.

### Step 5: Create a New Index in Splunk Enterprise

Before sending the Linux logs, make an index for keeping them.

- Go to splunk web interface
- Go to setting and search **Indexes**
- Go to **New Index**

<img width="1578" height="925" alt="image" src="https://github.com/user-attachments/assets/8435fad1-0b1e-44f3-b641-59ad49105ed7" />

- Create a new index called **linux_log** or whatever name you want to provide.

<img width="805" height="803" alt="image" src="https://github.com/user-attachments/assets/279af187-e066-4366-8450-19525f98f089" />


### Step 6: Configure the Receiving Port on Splunk Enterprise

Before completing the Universal Forwarder configuration, the Splunk Enterprise server must be configured to receive forwarded data.

- Go to splunk web interface
- Go to setting and search **Forwarding and receiving**
- Go to **Configure receiving**

<img width="944" height="430" alt="image" src="https://github.com/user-attachments/assets/6697b448-8eca-483c-a0e5-cd67bcf3e0b6" />

- Create **New Receiving Port** and save

<img width="1686" height="547" alt="image" src="https://github.com/user-attachments/assets/044d9ddb-d234-4fbe-bfb3-796c95272442" />

<img width="1884" height="473" alt="image" src="https://github.com/user-attachments/assets/9c5484f6-827c-48a3-a03e-c91e113a1e5f" />


Once completed, the Splunk Enterprise server is ready to receive data from Universal Forwarders.

### Step 7: Configure Log Monitoring

Create the **inputs.conf** file for configuring the logs that need to be monitored by the Universal Forwarder.

Edit the file:

```bash
nano /opt/splunkforwarder/etc/system/local/inputs.conf
```

Add the following configuration:

```bash
[monitor:///var/log]
index=linux_log 
sourcetype=syslog
disabled=false
```
The above configuration helps to monitor all logs in the **/var/log** directory and send them to the **linux_log** index.

### Step 8: Configure the Forwarding Destination

After that, set up the destination Splunk Enterprise server.

Open:

```bash
nano /opt/splunkforwarder/etc/system/local/outputs.conf
```

And add this following configuration:

```bash
[tcpout]
defaultGroup=default-autolb-group
[tcpout:default-autolb-group]
server=192.168.20.1:9997  ## Replace 192.168.20.1 with the IP address of your Splunk Enterprise server.
#[tcpout-server://192.168.132.128:9997]  ## It is optional. It is only used when you want to configure SSL, compression, acknowledgements, etc.
```

### Step 9: Restart the Universal Forwarder

Apply the new configuration by restarting the Universal Forwarder.

```bash
/opt/splunkforwarder/bin/splunk restart
```

Verify that the service is running successfully.

```bash
/opt/splunkforwarder/bin/splunk status
```

### Step 10: Verify Log Monitoring

To confirm that the configured log files are being monitored, execute:

```bash
/opt/splunkforwarder/bin/splunk list monitor
```

The output should display the monitored directories and log files.

### Step 11: Verify Data in Splunk Enterprise

Navigate to the Search & Reporting app in Splunk Enterprise and run the search below:

```bash
index=linux_log
```

If the Universal Forwarder has been configured properly, then the Linux log files should start coming up in the search results.

To check authentication logs use:

```bash
index=linux_log source="/var/log/auth.log" 
```

Or for monitoring syslog events use:

```bash
index=linux_log sourcetype=syslog
```

<img width="1345" height="888" alt="image" src="https://github.com/user-attachments/assets/538b45e0-2d9f-4cdc-9a80-38e141f0cf00" />

