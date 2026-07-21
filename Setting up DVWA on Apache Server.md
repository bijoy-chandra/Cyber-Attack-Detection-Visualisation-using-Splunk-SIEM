## Setting up DVWA on Apache Server

### The purpose behind setting up DVWA on apache server is to perform attack from other machine analyze different kinds of attack.

1. Download the DVWA from github [DVWA](https://github.com/digininja/DVWA)
2. Change your directory to this

```bash
cd /var/www/html/
```

3. Run this command to download the DVWA into **html** directory
```bash
sudo git clone https://github.com/digininja/DVWA.git
```

4. Give permission to DVWA directory and do the followings

```bash
sudo chmod -R 777 DVWA
cd /DVWA/config
sudo cp config.inc.php.dist config.inc.php
```

5. Change the following into the **config.inc.php** file

```bash
sudo nano config.inc.php
```
```bash
db_user:admin  ## Same username and password will be used in database creation
db_password:password  ## this credential will be used when you perform database setup for DVWA
```

<img width="1235" height="579" alt="image" src="https://github.com/user-attachments/assets/0958a11b-5c8c-4762-a04f-2e9fefd9b8c1" />


6. Note, MariaDB is default in Kali, so you can't use the database root user, you must create a new database user. To do this, connect to the database as the root user then use the following commands:

```bash
sudo systemctl start mysql
mysql -u root -p
create database dvwa;  ## OR the username you setup in **config.inc.php** file
create user dvwa@localhost identified by 'p@ssw0rd';  ## OR the password you setup in **config.inc.php** file
grant all on dvwa.* to dvwa@localhost;
flush privileges;
```

7. Restart the **Mysql** server and start **Apache** server

```bash
sudo systemctl restart mysql
sudo systemctl start apache2
```

8. Open the browser locate the directory into your host IP address. Login with your credential and then in the bottom of the website you will find **Reset Database** option. Reset the database and re-login in the site again, dvwa setup is completed.

```bash
http://192.168.132.130/DVWA
```


<img width="1269" height="721" alt="image" src="https://github.com/user-attachments/assets/d573c6fd-7df3-4188-b09b-64238750ddea" />


