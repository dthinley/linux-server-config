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
