## Linux Server Configuration
A project for a setup and configure a Linux (Ubuntu) web server using Amazon AWS. The server must be secure and serve an application previously developed in the course.

## Quick start
| Name | Value|
| --- | --- |
| IP Address/web address | 54.191.130.12 |
| SSH Port | 2200 |
| Username | grader|

To connect to EC2 instance you need the password (supplied separately in the submit process):

    ssh grader@54.191.130.122 -p 2200 -i ~/.ssh/keypair 

## Project setup
Start a new Ubuntu Linux Server instance on Amazon Lightsail at https://aws.amazon.com/lightsail/ and create an AWS account.
[Detail steps to steps guide can be found at](https://classroom.udacity.com/nanodegrees/nd004/parts/ab002e9a-b26c-43a4-8460-dc4c4b11c379/modules/357367901175462/lessons/3573679011239847/concepts/c4cbd3f2-9adb-45d4-8eaf-b5fc89cc606e)
Once the instance is up and running. You can ssh to log in as admin. Like Ubuntu@ip.

## 1. Create a new user named grader and grant this user sudo permissions

	$ sudo adduser grader

	$ sudo nano /etc/sudoers.d/grader
		
#### Include following text: "grader ALL=(ALL:ALL) ALL", then save it.

## 2. Update software packages on server instance
	$ sudo apt-get update

	$ sudo apt-get upgrade

## 3. Change SSH port to 2200 and configure access
	$ sudo nano /etc/ssh/sshd_config
Change the SSH port from 22 to 2200


## 4. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
	 $ sudo ufw allow 2200/tcp
	
 	 $ sudo ufw allow 80/tcp
 	
	 $ sudo ufw allow 123/udp
 	
	 $ sudo ufw enable
	
## 5. Configure the local timezone to UTC
	 $ sudo dpkg-reconfigure tzdata and then choose UTC
	 
## 6. Configure key-based authentication for grader user
1. Generate keys-pair on local machine using:
	
		$ ssh-keygen 

Detail guide in can be found:
[Detail guide in can be found:](https://classroom.udacity.com/nanodegrees/nd004/parts/ab002e9a-b26c-43a4-8460-dc4c4b11c379/modules/357367901175461/lessons/4331066009/concepts/48010894770923)

2 Login into grader account using sudo login grader. 
3. On your local machine, read the generated public key 
		
		$ cat /home/user/.ssh/keypair

4. Make a directory in grader account on your server: 

		$ mkdir .ssh
		$ touch .ssh/authorized_keys
		$ nano .ssh/authorized_keys
		
#### Copy the public key generated on your local machine to this file and save

		$ chmod 700 .ssh
		$ chmod 644 .ssh/authorized_keys
		$ sudo service ssh restart.
		ssh grader@54.191.130.122 -p 2200 -i ~/.ssh/keypair in new terminal.

For completing this section:[more details can be found at:](https://classroom.udacity.com/nanodegrees/nd004/parts/00413454014/modules/357367901175461/lessons/4331066009/concepts/48010894750923#)


## 7. Install and Configure Apache

	$ sudo apt-get install apache2
	
## 8. Install and configure Python mod_wsgi

	$ sudo apt-get install libapache2-mod-wsgi python-dev

#### Enable mod_wsgi:

	$ sudo a2enmod wsgi

#### Restart Apache2:
	
	$ sudo service apache2 start
	
## 9. Install git, clone and setup your Catalog App project.
	$ sudo apt-get install git
	$ cd /var/www
	$ sudo mkdir catalog

##### Change owner of the newly created catalog folder sudo chown -R grader:grader catalog
	$ cd /catalog

#### [Clone your project from github git clone](https://github.com/dthinley/linux-server-configt)

## 10. Configure .wsgi file

#### Create file: 

		$ sudo touch /var/www/catalog/Projectcatalog.wsgi

Create a catalog.wsgi file:
	
	#!/usr/bin/python
	import sys
	import logging
	logging.basicConfig(stream=sys.stderr)
	sys.path.insert(0,"/var/www/catalog/")

	from Projectcatalog import app as  application
	application.secret_key = 'super_secret_key'


#### Rename application.py to init.py mv catalog.py __init__.py

## 10. Install virtual environment
	Install the virtual environment sudo pip install virtualenv
	Create a new virtual environment with sudo virtualenv venv
	Activate the virutal environment source venv/bin/activate
	Change permissions sudo chmod -R 777 venv
	
## 11. Install Flask and other dependencies
	Install pip with sudo apt-get install python-pip
	Install Flask pip install Flask
	Install other project dependencies sudo pip install httplib2 oauth2client sqlalchemy psycopg2 sqlalchemy_utils
	
## 12. Update path of client_secrets.json file
	nano __init__.py
	Change client_secrets.json path to /var/www/catalog/catalog/client_secrets.json
	
## 13. Configure and enable a new virtual host
	$ sudo nano /etc/apache2/sites-available/Projectcatalog.conf

#### copy and paste this code:

	<VirtualHost *:80>
		ServerName 54.191.130.122
		ServerAdmin kyampo59@gmail.com
		WSGIScriptAlias / /var/www/catalog/Projectcatalog/Projectcatalog.wsgi
		<Directory /var/www/catalog/Projectcatalog/>
			Order allow,deny
			Allow from all
		</Directory>
		Alias /static /var/www/catalog/Projectcatalog/static
		<Directory /var/www/catalog/Projectcatalog/static/>
			Order allow,deny
			Allow from all
		</Directory>
		ErrorLog ${APACHE_LOG_DIR}/error.log
		LogLevel warn
		CustomLog ${APACHE_LOG_DIR}/access.log combined
	</VirtualHost>

#### Enable the virtual host 
	$ sudo a2ensite catalog
	
## 14. Install and configure PostgreSQL
	$ sudo apt-get install libpq-dev python-dev
	$ sudo apt-get install postgresql postgresql-contrib
	$ sudo su - postgres
### psql
#### CREATE USER catalog WITH PASSWORD ‘password’;
#### ALTER USER catalog CREATEDB;
#### CREATE DATABASE catalog WITH OWNER catalog;
#### \c catalog
#### REVOKE ALL ON SCHEMA public FROM public;
#### GRANT ALL ON SCHEMA public TO catalog;
### \q
### exit

Change create engine line in your __init__.py , and database_setup.py to: engine = create_engine(r’/var/www/catalog/catalog/client_secrets.json’, ‘r’).read())

### 15. Restart Apache
	$ sudo service apache2 restart
