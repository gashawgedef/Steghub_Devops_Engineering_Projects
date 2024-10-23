# Load Balancer Solution With Nginx and SSL/TLS

**Load balancing** : is an excellent way to scale out your application and increase its performance and redundancy. Nginx, a popular web server software, can be configured as a simple yet powerful load balancer to improve your server’s resource availability and efficiency.

**SSL/TLS Termination** : Generate an SSL certificate and private key for the domain name users will access. You can use a free Let's Encrypt certificate or obtain one from a certificate authority. Configure Nginx to use the SSL certificate and key within the server block listening on port 443.Nginx will handle the SSL/TLS encryption and decryption for incoming connections. Traffic between Nginx and the backend servers will be unencrypted HTTP.

**Task**

This project consists of two parts:

1. _Configure Nginx as a Load Balancer_
   
2. _Register a new domain name and configure secured connection using SSL/TLS certificates_
 
Our target architecture will look like this:

![image](assets/nginx.png)

# Part 1 Configure Nginx as a load balancer

1. Create an EC2 VM based on Ubuntu Server 24.04 LTS and name it nginx Ls 

  ![image](assets/nginx_1_instance_name.JPG)


Application and OS Images select Ubuntu free tire eligable version

![image](assets/nginx_os.JPG)

Create new key pair or select existing key

![image](assets/nginx_3_key_pair.JPG)

Network setting create new security group or use existing security group

![image](assets/nginx_4_.JPG)

Configure Storage and launch the instance

![image](assets/nginx_5_configure_storage.JPG)

View Instance

![image](assets/nginx_6_vew.JPG)

Instance Details for web

![image](assets/nginx_7_view_details.JPG)

Configure security group with the following inbound rules:

- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default.
- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.

![image](assets/nginx_6_open_ports.JPG)

Now connect to ssh terminal and work on

![image](assets/nginx_&_connect_servers.JPG)

2. Update `/etc/hosts` file for local DNS with Web Servers' names (e.g web1 and web2) and their local IP addresses
```
sudo vi /etc/hosts
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d9640441-cd4c-4313-a82c-e0e7c772b9bb)

Verify change 
```
sudo cat /etc/hosts
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/43c35e2a-d294-4244-b33f-17c9cb958a6d)


3. Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

Update the instance
```
sudo apt update
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/de989d70-3b2a-44c8-98d2-3e46d467bc0b)


Install Nginx 

```
sudo apt install nginx
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/008ade69-defd-4243-800b-1744b41e9906)


4. Edit the Nginx configuration file to set up load balancing
```
  sudo vi /etc/nginx/nginx.conf
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5105b1e9-4c44-466f-b133-2d281b999064)

5. insert the following configuration in `http` section

```
    upstream myproject {
       server Web1 weight=5;
       server Web2 weight=5;
    }

    server {
        listen 80;
        server_name ww.domain.com;

        location / {
            proxy_pass http://myproject;
        }
    }
    # comment out this line
    # include /ete/nginx/sites-enabled/

```

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/823b737c-7fa7-4878-8b82-4721f68398b6)


6. Test the Configuration

Before reloading Nginx, test the configuration to ensure there are no syntax errors
```
sudo nginx -t
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/17176c26-50c7-426e-8854-fa3904c70b2a)

7. Reload Nginx
If the configuration test is successful, reload Nginx to apply the changes

```
sudo systemctl reload nginx
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/84228a24-d31c-4f3c-92a6-fe2660f20b31)

8. Verify  the service is up and running
```
sudo systemctl status nginx
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c32a1c4e-b36a-4e66-9a8c-0b57f26a7fba)

# Part 2 - Register a new domain name and configure secured connection using SSL/TLS certificates
In order to get a valid SSL certificate we need to register a **new domain name**, we can do it using any Domain name registrar - a company that manages reservation of domain names. The most popular ones are: `Godaddy.com`, `Domain.com`, `Bluehost.com`

1. Register a new domain
   
- Select a domain registrar like GoDaddy, Domain.com, Bluehost or other for this time I need free domain for that 
  got to [https://www.cloudns.net/main/](https://www.cloudns.net/main/) and signup and register new domain
 ![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/981c1190-497d-4d92-91e0-2d3060016872)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7f5a23ee-08b1-4f50-a9aa-14cbf3e7a84f)

- Register your desired domain name  (e.g. .com, .net, .org, .edu, .info, .xyz or any other)
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/475e6e74-2cdb-45a9-a151-a3a616327e3b)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ab8f7e71-7f65-489c-9c57-a681ecca543e)

Now let us check using DNS Checker  [https://dnschecker.org/#A/www.tooling.dns-dynamic.net](https://dnschecker.org/#A/www.tooling.dns-dynamic.net) 

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/bec57229-c0c4-4e44-ae44-85ad868b7b9f)

   
2. Assign an Elastic IP to our Nginx LB server and associate our domain name with this Elastic IP
learn how to allocate an Elastic IP and associate it with an EC2 server [Elastic IP addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html)

**Allocate an Elastic IP Address:**

2.1 In the AWS Management Console, navigate to the EC2 Dashboard

2.2 Select `Elastic IPs` from the left-hand menu

2.3 Click `Allocate Elastic IP address` and follow the prompts
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/2f7fbd6f-9f58-476a-b4a2-c5805fae72a3)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b6704cf1-91b9-43af-81d1-d9e4db48203e)

**Associate Elastic IP with Your EC2 Instance:**

2.1. Select the allocated Elastic Ip

2.2 Click `Actions` and choose `Associate Elastic IP address`

2.3 Select your Nginx EC2 instance from the list and associate the Elastic IP

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/761574f6-44f0-4e65-868b-2741f1f19ace)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/f494332e-d060-41ec-a06d-38c3c347706a)


3. **Update DNS Settings**
   
3.1. Update A record in your registrar to point to Nginx LB using Elastic IP address
   
3.2 Log in to your domain registrar's control panel

3.3 Find the DNS settings for your domain

3.4 Add or update an A record to point to your Elastic IP address

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/dae3e40a-291b-4825-abd1-8c30cab38c28)


4. Configure Nginx to recognize your new domain name
Open the Nginx configuration file, typically located at `/etc/nginx/nginx.conf` or a specific site configuration file in `/etc/nginx/sites-available/` update your nginx.conf with server_name www.<your-domain-name.com instead of server_name www.domain.com

**our server name :** `www.tooling.dns-dynamic.net`

```
sudo vi /etc/nginx/nginx.conf
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c3a61893-996d-45d7-bf2e-99fe277aac70)

**Restart Nginx**
```
sudo systemctl restart nginx
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3d1623a1-a37b-405f-83ef-c50b15f51c13)


5. Install `certbot` and request for an SSL/TLS certificate

**verify snapd service is active and running**
```
sudo systemctl status snapd
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/400685c2-ca83-4ce3-9509-b658b0a3b8c6)

**Install certbot**
```
sudo snap install --classic certbot
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e4c5cb23-4fb4-4708-9da7-7d21d5e7685a)

Obtain SSL/TLS Certificates:just follow the certbot instructions you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from `nginx.conf` file so make sure you have updated it on step 4).

**Create a Symlink for Certbot:** Create a symbolic link to make Certbot easily executable
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/bde0bbac-1acc-47e5-8b66-fc04c6ebb0f9)

**Run Certbot to obtain and install the certificate**
```
sudo certbot --nginx
```
Follow the prompts to select your domain and complete the installation
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/761e52c6-25d5-49bd-b904-e52ffd3af8b1)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/efd6032e-c35b-44db-bf65-233f73c780a4)


6. Test secured access to your Web Solution by trying to reach
- Visit `https://<your-domain-name.com>` in your web browser
  
- Ensure you see a padlock icon indicating a secure connection
  
- Click the padlock icon to view the details of the SSL certificate

> You shall be able to access your website by using HTTPS protocol (that uses TCP port 443) and see a padlock pictogram in your browser's search string. Click on the padlock icon and you can see the details of the certificate issued for your website.

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0ca058c5-36a2-4545-9833-46042168f914)

7. Set up periodical renewal of your SSL/TLS certificate
By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently.

**Test Renewal in Dry-Run Mode**
```
sudo certbot renew --dry-run
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0da3f087-b5bc-4fa0-90d1-0ad6b652db00)

> Best pracice is to have a scheduled job that to run renew command periodically. Let us configure a `cronjob` to run the command twice a day

**Open the crontab editor**
```
crontab -e
```
**Add the following line to schedule the renewal command to run twice a day**:
```
* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b2a41e66-2c77-470a-8edb-9c274ade9087)

**Additional Resources**
- For more information on cron jobs, watch the video [Job Scheduling (cronjob/crontab) on Linux CentOS 8](https://www.youtube.com/watch?v=4g1i0ylvx3A)
-Use an online cron expression editor to help create and understand cron expressions [online cron expression editor](https://crontab.guru/)
