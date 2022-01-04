# Zabbix-Installation
**How to Install Zabbix Server 4.0 on Ubuntu 18.04**

Zabbix is an open source software for networks and application monitoring. Zabbix provides agents to monitor remote hosts as well as Zabbix includes support for monitoring via SNMP, TCP and ICMP checks. 

Step 1 – Install Apache, MySQL and PHP
You must have a LAMP environment on your server to use Zabbix. If you already have LAMP configured, just skip this step, else install Apache, MySQL, and PHP using the following commands.

sudo apt-get update
sudo apt-get install apache2 libapache2-mod-php
sudo apt-get install mysql-server
sudo apt-get install php php-mbstring php-gd php-xml php-bcmath php-ldap php-mysql

Update timezone in php configuration file /etc/php/PHP_VERSION/apache2/php.ini. Like below:

[Date]
; http://php.net/date.timezone
date.timezone = 'Asia/Kolkata'

Step 2 – Enable Required Apt Repository
## Ubuntu 18.04 LTS (Bionic):

wget https://repo.zabbix.com/zabbix/4.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.0-3+bionic_all.deb
sudo dpkg -i zabbix-release_4.0-3+bionic_all.deb

Step 3 – Install Zabbix Server
sudo apt-get update
sudo apt-get install zabbix-server-mysql zabbix-frontend-php zabbix-agent

Step 4 – Create Database Schema
Now create a database schema for your Zabbix server. Login to your MySQL server using administrative privileges and use the following queries to create MySQL database and user for the Zabbix server.

mysql -u root -p

mysql> CREATE DATABASE zabbixdb character set utf8 collate utf8_bin;
mysql> CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'password';
mysql> GRANT ALL PRIVILEGES ON zabbixdb.* TO 'zabbix'@'localhost' WITH GRANT OPTION;
mysql> FLUSH PRIVILEGES;

Also, load the Zabbix database schema to the database created above.

cd /usr/share/doc/zabbix-server-mysql
zcat create.sql.gz | mysql -u zabbix -p zabbixdb

Step 5 – Edit Zabbix Configuration File

Edit Zabbix server configuration file /etc/zabbix/zabbix_server.conf in your favorite text editor and update the following database configurations. This will be used by Zabbix server to connect to the database.

  DBHost=localhost
  DBName=zabbixdb
  DBUser=zabbix
  DBPassword=password
  
  Step 6 – Restart Apache and Zabbix
  
  sudo service apache2 restart
  sudo service zabbix-server restart
  
  Step 7 – Complete Zabbix Web Installer Wizzard
  
  http://server-ip-address/zabbix/
  
  Zabbix Setup Welcome Screen
This is the welcome screen of Zabbix web installer. Go forward by click on next button.



Check for pre-requisities
Check if your system has all required packages, if everything is ok click next.



Configure DB Connection
Enter database details created in Step #4 and click next to continue.



Zabbix Server Details
This is the host and port of running Zabbix server. As your Zabbix server is running on the same host, so keep the values unchanged. You can give a name for your instance.



Pre-Installation Summary
In this step will show the summary you have entered previous steps, so simply click next.



Install Zabbix
If everything goes correctly, You will see a successful installation message on this page. This will also show you a message for the created configuration file.



Zabbix Login Screen
Login to Zabbix using default credentials.

 Username:  Admin
 Password:  zabbix


After successful login, You will get Zabbix dashboard like below.

Ubuntu Install Zabbix server 

Congratulation! Your Zabbix setup has been completed.
  
