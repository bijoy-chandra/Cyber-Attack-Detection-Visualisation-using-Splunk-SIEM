## Installing Splunk Forwarder on Windows

This part explains how to set up the installation and configuration of the Splunk Universal Forwarder on Windows. This application receives data from Windows Event Logs and forwards them to the Splunk Enterprise Server for monitoring and analysis.

### Prerequisites

Before performing the installation process, please make sure that the following prerequisites are met:

- Splunk Enterprise software is installed and operational.
- The receiving port has been set up for the Splunk Enterprise server.
- There is network connectivity between the Windows machine and the Splunk server.
- The Windows machine has administrator privileges.

### Step 1: Download Splunk Universal Forwarder

- Visit the Splunk Universal Forwarder [Downloads page](https://www.splunk.com/en_us/blog/learn/splunk-universal-forwarder.html).
- Download the Splunk Universal Forwarder (64-bit) for Windows.
- Save the installer to your local machine.

 <img width="1871" height="939" alt="image" src="https://github.com/user-attachments/assets/93087ada-04db-4e29-bee2-4834554561de" />


### Step 2: To continue the installation, check the "Check this box to accept the License Agreement" checkbox. This activates the "Customize Installation" and "Next"

<img width="772" height="596" alt="image" src="https://github.com/user-attachments/assets/254136f4-844b-43ea-9117-abcd83a1c7fe" />


### Step 3.  Create Username and Password and then click next

<img width="765" height="597" alt="image" src="https://github.com/user-attachments/assets/e118986a-2aa8-42e0-939e-fd7e29b7b1c6" />

### Step 4: Configure the Receiving Port on Splunk Enterprise

Before completing the Universal Forwarder installation, the Splunk Enterprise server must be configured to receive forwarded data.

- Go to splunk web interface
- Go to setting and search **Forwarding and receiving**
- Go to **Configure receiving**

<img width="944" height="430" alt="image" src="https://github.com/user-attachments/assets/6697b448-8eca-483c-a0e5-cd67bcf3e0b6" />

- Create **New Receiving Port** and save

<img width="1686" height="547" alt="image" src="https://github.com/user-attachments/assets/044d9ddb-d234-4fbe-bfb3-796c95272442" />

<img width="1884" height="473" alt="image" src="https://github.com/user-attachments/assets/9c5484f6-827c-48a3-a03e-c91e113a1e5f" />


Once completed, the Splunk Enterprise server is ready to receive data from Universal Forwarders.


### Step 5: Configure the Receiving Indexer

- Enter the server IP you've hosted your splunk server and the listener port number.

<img width="771" height="601" alt="image" src="https://github.com/user-attachments/assets/8154b046-57a1-4760-859b-90f1a281e430" />

### Step 6: Complete the Installation

- Click install and allow "Yes" to continue installing splunk forwarder. After some time you'll see your splunk forwarder successfully installed and ready to collect windows logs

<img width="770" height="599" alt="image" src="https://github.com/user-attachments/assets/25368e47-351e-447e-8f42-8715e560ff80" />

### Step 7: Verify Windows Event Log Collection

Go to the Search & Reporting app in Splunk Enterprise and run the following search command:

```bash
index=*
```

If your Universal Forwarder is set up properly, you will see that there are Windows Event Logs.

You can test it again by running this search command:

```bash
index=* sourcetype=WinEventLog:*
```

You will get confirmation that your Windows is working correctly with Splunk.


<img width="1576" height="929" alt="image" src="https://github.com/user-attachments/assets/e30c87a0-181f-4d57-a807-7315374ce784" />




