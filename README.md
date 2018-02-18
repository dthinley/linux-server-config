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
Detail steps to steps guide can be found at 
https://classroom.udacity.com/nanodegrees/nd004/parts/ab002e9a-b26c-43a4-8460-dc4c4b11c379/modules/357367901175462/lessons/3573679011239847/concepts/c4cbd3f2-9adb-45d4-8eaf-b5fc89cc606e

Once the instance is up and running. You can ssh to log in as admin. Like Ubuntu@ip.

## 1. Create a new user named grader and grant this user sudo permissions

	$ sudo adduser grader

	$ sudo nano /etc/sudoers.d/grader
		
Include following text: "grader ALL=(ALL:ALL) ALL", then save it.

## 2. Update software packages on server instance
	$ sudo apt-get update

	$ sudo apt-get upgrade

## 3. Change SSH port to 2200 and configure access
	$ sudo nano /etc/ssh/sshd_config
Change the SSH port from 22 to 2200


## 4. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
	* $ sudo ufw allow 2200/tcp
 	* $ sudo ufw allow 80/tcp
 	* $ sudo ufw allow 123/udp
 	* $ sudo ufw enable
	
## 5. Configure the local timezone to UTC
Run sudo dpkg-reconfigure tzdata and then choose UTC



