## Installing Splunk Forwarder on Windows

1. Download Splunk universal forwarder 64 bit

 <img width="1871" height="939" alt="image" src="https://github.com/user-attachments/assets/93087ada-04db-4e29-bee2-4834554561de" />


3. To continue the installation, check the "Check this box to accept the License Agreement" checkbox. This activates the "Customize Installation" and "Next"

<img width="772" height="596" alt="image" src="https://github.com/user-attachments/assets/254136f4-844b-43ea-9117-abcd83a1c7fe" />


4.  Create Username and Password and then click next

<img width="765" height="597" alt="image" src="https://github.com/user-attachments/assets/e118986a-2aa8-42e0-939e-fd7e29b7b1c6" />

Now before we move into step 5, we have do some addition setting into our splunk server.

==> Go to splunk web interface
==> Go to setting and search **Forwarding and receiving**
==> Go to **Configure receiving**

<img width="944" height="430" alt="image" src="https://github.com/user-attachments/assets/6697b448-8eca-483c-a0e5-cd67bcf3e0b6" />

==> Create **New Receiving Port** and save

<img width="1686" height="547" alt="image" src="https://github.com/user-attachments/assets/044d9ddb-d234-4fbe-bfb3-796c95272442" />

<img width="1884" height="473" alt="image" src="https://github.com/user-attachments/assets/9c5484f6-827c-48a3-a03e-c91e113a1e5f" />


Now the listener is setup for splunk, the next step to go back splunk forwarder configuration and set the rest.

5. Enter the server IP you've hosted your splunk server and the listener port number.

<img width="769" height="596" alt="image" src="https://github.com/user-attachments/assets/02eacace-842b-4545-8f38-7d9341fcd185" />

6. Enter the host IP address and port number. In my case my windows IP is same as my splunk server IP address.

<img width="771" height="601" alt="image" src="https://github.com/user-attachments/assets/8154b046-57a1-4760-859b-90f1a281e430" />

7. Then click install and allow "Yes" to continue installing splunk forwarder. After some time you'll see your splunk forwarder successfully installed and ready to collect windows logs

<img width="770" height="599" alt="image" src="https://github.com/user-attachments/assets/25368e47-351e-447e-8f42-8715e560ff80" />



