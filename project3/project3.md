# Project 3: Setting up Load Balancing for Static Website Using Nignx

## Introduction

In this project, we aim to show Layer 7 load balancing and load balancing techniques using Nginx as a load balancer.

## Checklist

- [x] Task 1: Deploy three servers.
- [x] Task 2: Set up static websites on two servers using Nginx.
- [x] Task 3: Make a small change in the index.html file of one of the websites to differentiate between two servers OR For a clearer distinction, use two separate HTML files with distinct content.
- [x] Task 4: Set up Nginx on the third server. It will act as a load balancer.
- [x] Task 5: Configure Nginx to load and balance traffic between two static websites.
- [x] Task 6: Add the Nginx Load balancer IP to the DNS A record.
- [x] Task 7: Try accessing the website. Every time you reload the website you should see a different index.html.


## Documentation

### Setting up an Ubuntu Server
- Login into your AWS Account as a **Root User** 
- Search and click on **EC2** within the AWS management console
- Click on **Launch instance** button
- Give a title to the instance using the **Name** field and then select the **Ubuntu** AMI from the **Quick Start** options
- Scroll down to **Key pair (login)** and click on **Create new key pair** button to generate a key pair for secure connection to your instance.
- Input a **Key pair name** and click on **Create key pair**
- On the **Network settings**, select the boxes for each of the follow;  **SSH**, **HTTP**, and **HTTPS** access, then click **Launch instance**
- Repeat for your second server and load balancer, making sure to label appropriately

![1](img/image1.png)

## Install Nginx and Setup Your Website
# Execute the following commands.
- **`sudo apt update`**  
- **`sudo apt upgrade`**
- **`sudo apt install nginx`**

### Use the **`sudo systemctl start nginx`** command to launch your Nginx server. Then, use the **`sudo systemctl enable nginx`** command to make sure it runs upon boot. Finally, use the **`sudo systemctl status nginx`** command to verify that it is up and running.

* To see the default Nginx launch page, open a web browser and navigate to your instance's IP address.

![2](img/image2.png)

![3](img/image3.png)


* Go to the website of your choice, find the template you want, and download your website template..
* Choose Inspect from the drop-down menu by performing a right-click.
* After selecting the Network tab, click the Download option.
* Right click on the website name, select Copy and click on Copy link address.
* To install the unzip tool, run the following command: **`sudo apt install unzip`**
* Execute the command to download and unzip your website files **`sudo curl -o /var/www/html/2100_artist.zip https://www.tooplate.com/zip-templates/2100_artist.zip && sudo unzip -d /var/www/html/ /var/www/html/2100_artist.zip && sudo rm -f /var/www/html/2100_artist.zip`**~
* Download the second template for the website and run the above command again.
* Execute the command to download and unzip your website files **`sudo curl -o /var/www/html/2088_big_city.zip https://www.tooplate.com/zip-templates/2088_big_city.zip && sudo unzip -d /var/www/html/ /var/www/html/2088_big_city.zip && sudo rm -f /var/www/html/2088_big_city.zip`**~
* To set up your website's configuration, create a new file in the Nginx sites-available directory. 
Use this command to open a blank file in a text editor: **`~sudo nano /etc/nginx/sites-available/artist~`**
* Edit the root directive within your server block to point to the directory where your downloaded website content is stored.
* Copy and paste the following code into the open text editor. 

server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/html/example.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
    }

* Edit the root directive within your server block to point to the directory where your downloaded website content is stored.
* Follow the same steps for the secind website by creating a new file in the Nginx sites-available directory.
* Then create a symbolic link for both websites by running the following command.

+ **`sudo ln -s /etc/nginx/sites-available/artist /etc/nginx/sites-enabled/`** 
+ **`sudo ln -s /etc/nginx/sites-available/big_city /etc/nginx/sites-enabled/`**

* Run the **`sudo nginx -t`** command to check the syntax of the Nginx configuration file.
* Delete the default files in the sites-available and sites-enabled directories by executing the following commands:

 **`sudo rm /etc/nginx/sites-available/default`**
 **`sudo rm /etc/nginx/sites-enabled/default`**

* Restart the Nginx server by executing the following command: **`sudo systemctl restart nginx`**

![4](img/image4.png)


![5](img/image5.png)

## Configure your Load balancer
* Install Nginx on the server chosen to serve as a load balancer, and execute **`sudo systemctl status nginx`** to ensure it's running.
* Execute **`sudo nano /etc/nginx/nginx.conf`** to edit your Nginx configuration file.
+ Add the following within the http block.

    
    upstream nextg {
    server 1;
    server 2;
    * Add more servers as needed
    }

server {
    listen 80;
    server_name <your domain> www.<your domain>;

    location / {
        proxy_pass http://nextg;
    }
    }

![6](img/image6.png)


* Run **`sudo nginx -t`** to check for syntax error.
* Restarting Nginx **`sudo systemctl restart nginx`**

### Create An A Record

You must configure a DNS record if you want people to reach your website using your domain name instead of its IP address. A domain was purchased on Namecheap, transferred my hosting to AWS Route 53, and created an A record there

## Point your domain's A records to the IP address of your Nginx load balancer server.
- On the website select **Domain List**
- Click on the **Manage** button
- Log into your AWS console, search  and select **Route 53** from the list of services displayed
- Click on **Get started**
- Select **Create hosted zones** and click on **Get started**
- Input your **Domain name**, choose **Public hosted zone** and then click on **Create hosted zone**
- Select the **created hosted zone①** and copy the assigned **Values**
- Return to your domain registrar and select **Custom DNS** within the **NAMESERVERS** section
- Paste the values copied from Route 53 into the appropriate fields, then select the **checkmark symbol** to save the changes done
- Head back to your AWS console and click on **Create record**
- Paste your Elastic IP address and then click on **Create records**
- Your A record has been successfully created
- Click on **create record** again, to create the record for your sub domain
- Input the Record name(**www**), paste your **IP address**, and then click on **Create records**.

- Open your terminal for the first server and run **`sudo nano /etc/nginx/sites-available/artist`** to edit your settings. Enter the name of your domain and then save your settings.
- Run **`sudo nano /etc/nginx/sites-available/big_city`** on the second web server terminal and to edit your settings. Enter the name of your domain and then save your settings.
- Restart your nginx server by running the **`sudo systemctl restart nginx`** command.
- To make sure your website is reachable, open a web browser and navigate to your domain name.

### Install certbot and Request For an SSL/TLS Certificate

- Install certbot by executing the following commands on our load balancer terminal:
**`sudo apt update`**
**`sudo apt install certbot python3-certbot-nginx`**
- Execute the **`sudo certbot --nginx`** command to request your certificate. Follow the instructions provided by certbot and select the domain name for which you would like to activate HTTPS

![7](img/image7.png)

* Access your website to verify that Certbot has successfully enabled HTTPS.

![8](img/image8.png)

---
---

#### The End Of Project 3