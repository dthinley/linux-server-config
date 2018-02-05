## Linux-server-config
A project for a setup and configure a Linux (Ubuntu) web server using Amazon AWS. The server must be secure and serve an application previously developed in the course.

## Table of contents
1. Quick start
2. Summary of software and configuration
3. Securing server
4. User management
5. Third-Party Resources
6. Creator


## Server Instance setup using AWS Lightsail
Set up an Ubuntu server instance in AWS Lightsail as per Udacity instructions.

## Add new user with sudo privileges
Once you have connected to your Lightsail instance via SSH, type the following into the command line interface to create a new user named 'grader'. Pssword for this instance is 'Gr@der1'.
$ sudo adduser grader
Grant the user 'grader' sudo privileges using the following command:
$ sudo visudo
This will open up the sudo configuration file. Add the following line below 'root ALL=(ALL:ALL) ALL':

grader ALL=(ALL:ALL) ALL

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

## Set up automatic security updates
Note: This is an extra requirement of the project. However, in a real life, critical application I would not have enabled automatic upgrading of packages. In the interest of stability, upgrages would be applied manually after careful evaluation. We would then phase un upgrades in a dev machine(s) before pushing into production.

We will use the unattended-upgrades package.

sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
This opens a console application that prompts the user. Select "yes".

Source Ubuntu doecumentation on AutomaticSecurityUpdates.

### Fix warning sudo: unable to resolve host ip-xx-yy-zz-xyz
Hostname is in /etc/hostname. Add that hostname to /etc/hosts:

127.0.1.1 ip-xx-yy-zz-xyz
And reboot the machine
