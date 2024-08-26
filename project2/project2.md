# Project 1 Title: Setup Multiple Static Websites on a Single Server Using Nginx Virtual Hosts

## Introduction

In this project, we are going to learn about subdomains and how to set up Nginx Virtual Host to host several websites on a single server.

## Checklist

- [x] Task 1: Buy a domain name from a domain Registrar.
- [x] Task 2: Spin up a Ubuntu server & assign an elastic IP to it.
- [x] Task 3: SSH into the server and install Nginx.
- [x] Task 4: Find freely available HTML website files.
- [x] Task 5: Download and unzip the website files to the Nginx website directory.
- [x] Task 6: Validate the website using the server IP address.
- [x] Task 7: In Route53, create an A record and add the Elastic IP.
- [x] Task 8: Using DNS verify the website setup.
- [x] Task 9: Install certbot and Request For an SSL/TLS Certificate.
- [x] Task 10: Validate the website SSL using the OpenSSL utility.

## Documentation

### Setting up an Ubuntu Server
- Login into your AWS Account as a **Root User** 
- Search and click on **EC2** within the AWS management console
- Click on **Launch instance** button
- Give a title to the instance using the **Name** field and then select the **Ubuntu** AMI from the **Quick Start** options
- Scroll down to **Key pair (login)** and click on **Create new key pair** button to generate a key pair for secure connection to your instance.
- Input a **Key pair name** and click on **Create key pair**
- On the **Network settings**, select the boxes for each of the follow;  **SSH**, **HTTP**, and **HTTPS** access, then click **Launch instance**

![1](image)

## Install Nginx and Setup Your Website
# Execute the following commands.
- sudo apt update
- sudo apt upgrade
- sudo apt install nginx

### Use the sudo systemctl start nginx command to launch your Nginx server. Then, use the sudo systemctl enable nginx command to make sure it runs upon boot. Finally, use the sudo systemctl status nginx command to verify that it is up and running.

* To see the default Nginx launch page, open a web browser and navigate to your instance's IP address.
* Go to the website of your choice, find the template you want, and download your website template..
* Choose Inspect from the drop-down menu by performing a right-click.
* After selecting the Network tab, click the Download option