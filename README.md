## Linux Server Configuration
A project for a setup and configure a Linux (Ubuntu) web server using Amazon AWS. The server must be secure and serve an application previously developed in the course.

## Summary for project setup
1. Start a new Ubuntu Linux Server instance on Amazon Lightsail at https://aws.amazon.com/lightsail/ and create an AWS account
2. Click Create instance button on the home page and select Linux/Unix platform, and select OS Only and Ubuntu as blueprint

![](images/screen1.jpg)

### Select an instance plan

![](images/screen2.jpg)

3. Name your instance
4. Click Create button. (Creating instance will take few seconds and will soon be up and running)

Once the instance is up and running. You can ssh to log in as admin. Like Ubuntu@ip.

## Create a new user named grader and grant this user sudo permissions

	sudo adduser grader

## Add new user with sudo privileges
Once you have connected to your Lightsail instance via SSH, type the following into the command line interface to create a new user named 'grader'. Pssword for this instance is 'Gr@der1'.
$ sudo adduser grader
Grant the user 'grader' sudo privileges using the following command:
$ sudo visudo
This will open up the sudo configuration file. Add the following line below 'root ALL=(ALL:ALL) ALL':

grader ALL=(ALL:ALL) ALL

## Quick start
| Name | Value|
| --- | --- |
| IP Address | 54.191.130.12 |
| SSH Port | 2200 |
| Username | grader|

To connect to EC2 instance you need the password (supplied separately in the submit process):

    ssh grader@54.191.130.122 -p 2200 -i ~/.ssh/keypair 


2. Summary of software and configuration
3. Securing server
4. User management
5. Third-Party Resources
6. Creator

## Server Instance setup using AWS Lightsail
Set up an Ubuntu server instance in AWS Lightsail as per Udacity instructions.

## Add new user with sudo privileges
1. Add a new user called grader: 
	
		$ sudo adduser grader

		$ sudo nano /etc/sudoers.d/grader
		
Include following text: "grader ALL=(ALL:ALL) ALL", then save it.

## Update software packages on server instance
Enter the following lines into the command line interface:
$ sudo apt-get update

$ sudo apt-get upgrade

## Change SSH port to 2200 and configure access
Enter the following command to access the server's SSH configuration file:
$ sudo nano /etc/ssh/sshd_config
Change the SSH port from 22 to 2200
Change PermitRootLogin without-password to PermitRootLogin no.
Change PasswordAuthentication from yes to no (this is only temporary).
Add the following to the end of the file:
UseDNS no
AllowUsers grader

Setup and enble a virtual host
Create file: $ sudo touch /etc/apache2/sites-available/catalog.conf
Add the following to the file:
   <VirtualHost *:80>
		ServerName XX.XX.XX.XX
		ServerAdmin admin@xx.xx.xx.xx
		WSGIScriptAlias / /var/www/catalog/catalog.wsgi
		<Directory /var/www/catalog/catalog/>
			Order allow,deny
			Allow from all
			Options -Indexes
		</Directory>
		Alias /static /var/www/catalog/catalog/static
		<Directory /var/www/catalog/catalog/static/>
			Order allow,deny
			Allow from all
			Options -Indexes
		</Directory>
		ErrorLog ${APACHE_LOG_DIR}/error.log
		LogLevel warn
		CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
Run $ sudo a2ensite catalog to enable the virtual host
Restart Apache: $ sudo service apache2 reload
Configure .wsgi file
Create file: $ sudo touch /var/www/catalog/catalog.wsgi
Add content below to this file and save:
   #!/usr/bin/python
   import sys
   import logging
   logging.basicConfig(stream=sys.stderr)
   sys.path.insert(0,"/var/www/nuevoMexico/")

   from nuevoMexico import app as application
   application.secret_key = 'super_secret_key'
Restart Apache: $ sudo service apache2 reload
Edit the database path
Replace lines in __init__.py, database_setup.py, and lotsofitems.py with engine = create_engine('postgresql://catalog:INSERT_PASSWORD_FOR_DATABASE_HERE@localhost/catalog')
Disable defualt Apache page
$ sudo a2dissite 000-defualt.conf
Restart Apache: $ sudo service apache2 reload
Set up database schema
Run $ sudo python database_setup.py
Run $ sudo python lotsofitems.py
Restart Apache: $ sudo service apache2 reload
