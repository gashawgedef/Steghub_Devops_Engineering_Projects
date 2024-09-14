# Web stack(LAMP) Implimentation IN AWS

LAMP is an acronym that stands for four open-source software components commonly used to build dynamic websites and web applications:

- **Linux**: The operating system that provides the foundation for the server environment.
- **Apache HTTP Server**: The web server software that processes incoming requests from web browsers and delivers the requested content (HTML, images, etc.).
- **MySQL**: The relational database management system (RDBMS) that stores website data. MySQL offers a structured way to organize and manage website content, user information, application data, and more.
- **PHP (or Perl/Python)**: The server-side scripting language used to create dynamic web pages. PHP scripts can interact with the MySQL database to retrieve or manipulate data, generate customized content based on user input, and perform various server-side tasks. Perl and Python can also be used as alternatives in LAMP stacks, although PHP is the most common choice.

1. ### aws account setup and provisioning an Ubuntu Server

---

create Aws aacount to access aws console to run over all action Ec2
**EC2 Dashboard**
![EC2 Doashboard](assets/ec2-dashboard.jpg)

**Instance summary**
![Instance Summary Image](assets/instance-summary.jpg)

2. ### Connect to Ec2 Instance

---

First get private key[gashaw_key.pem] of EC2 instance server by download and give permission

```
chmod 400 "gashaw_key.pem"
```

The connect the instance using instance Public Ip Addresss

```
ssh -i melkamu_key.pem ubuntu@34.229.199.16
```
![image](assets/connect-instance.jpg)

3. ### Install Apache and Updating Firewall

---

```
sudo apt update
```

```
sudo apt install apache2
```

**Verify Installation**

```
sudo systemctl status apache2
```

![Verify Instalation](assets/verify-instalation.jpg)

**Check acccess locally**

```
curl http://localhost:80
```

```
curl http://127.0.0.1:80
```

**Add Inbound Rule using Port 80**
![Set Inbound Rules using port 80](assets/inbound-rule-80.jpg)

**Test in browser using public IP Address**

```
http://3.88.103.206/
```

![In Browser](assets/default-page.jpg)

4. ### Installing MYSQL

---

```
sudo apt install mysql-server
```
