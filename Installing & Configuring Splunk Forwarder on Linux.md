## Installing & Configuring Splunk Forwarder on Linux

1. Check what kernal type and version you are going to install splunk forwarder
 
 ```bash
uname -a
uname -r
```

In my case i'm using linux amd64

<img width="1067" height="624" alt="image" src="https://github.com/user-attachments/assets/7725f6a1-4c3b-49d1-b738-5e39c1ca24bd" />

2. Download splunk forwarder for linux amd64 debian **.deb** package. Click **Copy wget link** and paste it into your linux terminal.

<img width="1196" height="852" alt="image" src="https://github.com/user-attachments/assets/4c503bab-67fa-45cb-9ffb-c32f9dfc07aa" />

3. After downloading forwarder then run the following command to install splunk forwarder

```bash
sudo apt update
sudo apt install ./splunkforwarder-10.4.1-5a009d941268-linux-amd64.deb
/opt/splunkforwarder/bin/splunk start --accept-license --answer-yes
```

It will ask you to provide username and password. Provide your credential and enter for proceeding next step. As you can see splunk forwarder has been installed:

<img width="976" height="685" alt="image" src="https://github.com/user-attachments/assets/6e8d0154-d343-4a0e-adef-fdd7bddc6a10" />

Check that if **splunkfwd** is in adm group or not
```bash
getent group adm
```
If the **splunkfwd** is in adm group then you can move forward, but if it is not then run this command
```bash
sudo usermod -aG adm splunkfwd
```

Now before we move into step 4, we have do some addition setting into our splunk server.

==> Go to splunk web interface
==> Go to setting and search **Indexes**
==> Go to **New Index**

<img width="1578" height="925" alt="image" src="https://github.com/user-attachments/assets/8435fad1-0b1e-44f3-b641-59ad49105ed7" />

==> Create new index by giving a name to your index **linux_log** and save, no more additional changes requires for this.

<img width="805" height="803" alt="image" src="https://github.com/user-attachments/assets/279af187-e066-4366-8450-19525f98f089" />


Previously we setup listener (9997) in splunk server while setting splunk forwarder for windows, the next step to go back splunk forwarder configuration and set the rest.


4. Next step is configuring splunk **inputs.conf** and **outputs.conf** file for monitoring linux logs

change the following into the **inputs.conf** file

```bash
nano /opt/splunkforwarder/etc/system/local/inputs.conf
```

```bash
[monitor:///var/log]
index=linux_log 
sourcetype=syslog
disabled=false
```

change the following into the **outputs.conf** file

```bash
nano /opt/splunkforwarder/etc/system/local/outputs.conf
```

```bash
[tcpout]
defaultGroup=default-autolb-group
[tcpout:default-autolb-group]
server=192.168.20.1:9997  ## Splunk Server IP address
#[tcpout-server://192.168.132.128:9997]  ## It is optional. It is only used when you want to configure SSL, compression, acknowledgements, etc.
```

5. Now restart your splunk forwarder
```bash
/opt/splunkforwarder/bin/splunk restart
/opt/splunkforwarder/bin/splunk status
```

6. Check if the files are being monitored
```bash
/opt/splunkforwarder/bin/splunk list monitor
```


After setting up everything you can see linux logs are coming into splunk

<img width="1345" height="888" alt="image" src="https://github.com/user-attachments/assets/538b45e0-2d9f-4cdc-9a80-38e141f0cf00" />

