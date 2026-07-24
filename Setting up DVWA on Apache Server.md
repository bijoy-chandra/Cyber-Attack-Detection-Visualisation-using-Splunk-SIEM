## Setting up DVWA on Apache Server

## Introduction

The Damn Vulnerable Web Application (DVWA) is a deliberately vulnerable PHP/MySQL web application built for security experts, penetration testers, and students to test security on web applications in a controlled lab environment.

As the name suggests, DVWA will act as the target web application for performing web-based attacks like XSS in this project. The logs created by the web server will be collected using Splunk Universal Forwarder and analyzed in Splunk SIEM.


### Prerequisites

Before installing DVWA, ensure the following components are installed:

- Apache Web Server  (**apache2** default installed on kali)
- PHP
- MariaDB (or MySQL)
- Git
- Splunk Universal Forwarder is installed

### Step 1: Download DVWA

- Download the DVWA from github [DVWA](https://github.com/digininja/DVWA)
- Navigate to the Apache web root directory.

```bash
cd /var/www/html/
```

Clone the official DVWA repository from GitHub

```bash
sudo git clone https://github.com/digininja/DVWA.git
```

Give permission to DVWA directory and do the followings

```bash
sudo chmod -R 777 DVWA  ## Avoid using chmod -R 777 in production environments unless absolutely necessary.
```

### Step 2: Configure DVWA

Navigate to the configuration directory.

```bash
cd /DVWA/config
```

Create the configuration file.

```bash
sudo cp config.inc.php.dist config.inc.php
```

Edit the configuration file:

```bash
sudo nano config.inc.php
```

Modify the database configuration according to the credentials created below in the database.

```bash
$_DVWA['db_user'] = 'dvwa';  ## Same username and password will be used in database creation
$_DVWA['db_password'] = 'p@ssw0rd';  ## this credential will be used when you perform database setup for DVWA
```

Important! The database credentials set above have to be exactly the same as the one created in MariaDB.

<img width="1235" height="579" alt="image" src="https://github.com/user-attachments/assets/0958a11b-5c8c-4762-a04f-2e9fefd9b8c1" />


### Step 3: Create the Database

Note: MariaDB is default in Kali, so you can't use the database root user, you must create a new database user. To do this, connect to the database as the root user then use the following commands:

- Start the MariaDB service.
- Log in as the database administrator.
- Create the database and a dedicated user for DVWA.

```bash
sudo systemctl start mysql
mysql -u root -p
create database dvwa;  ## OR the username you setup in **config.inc.php** file
create user dvwa@localhost identified by 'p@ssw0rd';  ## OR the password you setup in **config.inc.php** file
grant all on dvwa.* to dvwa@localhost;
flush privileges;
```

### Step 4: Start Apache and MariaDB (MySql)

```bash
sudo systemctl restart mysql
sudo systemctl start apache2
```

Verify both services are running.

```bash
sudo systemctl status mysql
sudo systemctl status apache2
```

### Step 5: Complete the DVWA Installation

Open a web browser and navigate to the DVWA application.

```bash
http://<Server-IP>/DVWA  ## Example: http://192.168.132.130/DVWA
```

The DVWA setup page will appear.

Click Create / Reset Database to initialize the application database.

Once the database has been created successfully, log in using the default DVWA credentials:
- Username: admin
- Password: password

**Note: These are the default DVWA application credentials and are separate from the MariaDB database user created earlier.**


<img width="1269" height="721" alt="image" src="https://github.com/user-attachments/assets/d573c6fd-7df3-4188-b09b-64238750ddea" />


