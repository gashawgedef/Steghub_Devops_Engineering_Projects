
# MEAN Stack Development

The **MEAN** stack is a full-stack web development framework consisting of **MongoDB**, **Express.js**, **Angular**, and **Node.js**, all based on JavaScript. MongoDB serves as the NoSQL database, while Express.js and Node.js manage the server-side logic, with Angular providing the dynamic front-end interface.

This stack enables developers to build modern, scalable applications using a single language for both front-end and back-end, streamlining the development process. It is particularly suitable for developing single-page applications, leveraging Angular's two-way data binding for efficient UI updates and Node.js's asynchronous nature for improved performance.

## MEAN Web Stack Components: 

- **MongoDB:** A document-based, NoSQL database used to store application data in the form of documents.
- **Express.js:** A server-side web application framework for Node.js.
- **Angular:** A frontend framework developed by Google, based on TypeScript, used to build dynamic and responsive User Interface (UI) components.
- **Node.js:** A JavaScript runtime environment used to run JavaScript on a machine rather than in a browser.

## Key Benefits:
- Single language (JavaScript) across the stack.
- Highly scalable and suitable for modern web applications.
- Efficient, with Angular's two-way data binding for seamless UI updates and an asynchronous, event-driven backend using Node.js.

## Getting Started a MEAN STACK Project

**1. Launch instance in AWS Console**
The first step in implementing this project is to launch an instance in the AWS Console.

create an instance in the default region us-east-1 and enter instance name **MEAN STACK WEB SERVER**

![image](assets//1_launch_instance.JPG)
- Next we select ubuntu operating system for an instance
  
![image](assets/2_instance_os.JPG)

- Create or use existing  private key pair to log into the instance created
  
![image](assets/3_get_existing_key_pair.JPG)

- Choose the **volume size** and **type** for the instance created

![image](assets/5_configure_storage.JPG)

- Configuring the security group in AWS EC2
  
A security group in AWS acts as a virtual firewall for your EC2 instances. It controls both inbound and outbound traffic to ensure only the permitted traffic reaches your instance. Each EC2 instance must be associated with at least one security group. The security group rules can be customized to define the type of traffic that is allowed to connect to the instance, including protocols, ports, and IP addresses.

![image](assets/4_create_security_group.JPG)

- View the status of instance created

  ![image](assets/6_view_instance.JPG)

- View Instance Details

  ![image](assets/7_instance_details.JPG)

- Configuring Security Group with Specific Inbound Rules
  When setting up a security group for your EC2 instance, you can control which traffic reaches your instance through inbound rules.

For SSH (port 22), this rule allows secure shell access to your instance. By default, SSH is open to any IP address (0.0.0.0/0), which is useful for testing but insecure for production. It's recommended to restrict this access to trusted IPs to reduce exposure to unauthorized login attempts.

For HTTP (port 80), this rule allows web traffic from anywhere on the internet. This is essential if you're hosting a public-facing website or web service that users can access over HTTP.

For HTTPS (port 443), this rule enables secure web traffic. Like HTTP, HTTPS traffic is generally allowed from anywhere on the internet, ensuring encrypted access for your users. This is crucial for secure communication, especially for websites dealing with sensitive data.

By configuring these rules, your instance will allow SSH access for management and handle both HTTP and HTTPS traffic, making it accessible to the public while still maintaining necessary security measures.

  ![image](assets/configure%20inbound_rules.JPG)
- Connect to instance from ssh client

   ![image](assets/9_connect_instance_command.JPG)
**Give Permission for the Private SSH Key**
  
  This command ensures that this  private SSH key has the correct permissions before using it to connect to your instance.

  ```
  chmod 400 "gashaw_key.pem"
  ```
  ![image](assets/8_give_permission.JPG)
**Connecting to the Instance via SSH**

Once the private key file has the correct permissions, you can use SSH to connect to your EC2 instance using its public IP address or domain name.
```
ssh -i "gashaw_key.pem" ubuntu@ec2-54-226-142-99.compute-1.amazonaws.com
```
![image](assets/10_inatance_connected.JPG)


## Step 1 - Install NodeJs

- Update ubuntu

```
sudo apt update
```

![image](assets/11_update_ubunt.JPG)

- Upgrade Ubuntu

```
 sudo apt upgrade
```

![image](assets/12_upgrade_ubuntu.JPG)

- Add Certificates

```
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
```

![image](assets/13_certificate_1.JPG)


Lets get the location of Node.js software from Ubuntu repositories.

```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

![image](assets/15_certificates_2.JPG)


- **Install Node.js on the server**

```
sudo apt install -y nodejs
```

![image](assets/21_install_node_js.JPG)

- Verify the node installation with the command below

```
node -v 
```

## Step 2: Install MongoDB

MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

- **Install GnuPG and Curl**

This command installs essential tools for handling GPG keys and transferring data:

```
sudo apt-get install -y gnupg curl
```
![image](assets/50_install_gnu.JPG)

- **Download and Store MongoDB GPG Key**

This command downloads MongoDB’s GPG key and stores it securely for use in package verification:

```
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
```

![image](assets/16_install_mongo_1.JPG)

The command below adds the MongoDB repository to your system's package manager sources, enabling the installation and updates of MongoDB packages.

```
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

![image](assets/17_install_mongo_2.JPG)


- Install MongoDB

```
sudo apt-get install -y mongodb-org
```
![image](assets/18_install_mongo_db_main.JPG)

- Start The server

```
sudo systemctl start mongod

```

![image](assets/19_start_mongo_db.JPG)

- Verify that the service is up and running

```
sudo systemctl status mongod
```

![image](assets/23_check_system_is%20_running.JPG)

- Install [npm](https://www.npmjs.com) - Node package manager.


```
sudo apt install -y npm
```

![image](assets/51_install_node_package_manager.JPG)

- Install 'body-parser package

We need 'body-parser' package to help us process JSON files passed in requests to the server.

```
sudo npm install body-parser
```

![image](assets/25_install_parsor.JPG)

- Create a folder named 'Books'

```
mkdir Books && cd Books

```
![image](assets/26_create_and_goto_books.JPG)


- In the Books directory, Initialize npm project

```
npm init
```

![image](assets/27_npm_init.JPG)

- Add a file to it named server.js

```
vim server.js

```

![image](assets/28_open_server_js.JPG)

-
- Copy and paste the web server code below into the server.js file.

```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const path = require('path');

const app = express();
const PORT = process.env.PORT || 3300;

// MongoDB connection
mongoose.connect('mongodb://localhost:27017/test', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
.then(() => console.log('MongoDB connected'))
.catch(err => console.error('MongoDB connection error:', err));

app.use(express.static(path.join(__dirname, 'public')));
app.use(bodyParser.json());

require('./apps/routes')(app);

app.listen(PORT, () => {
  console.log(`Server up: http://localhost:${PORT}`);
});

```

![image](assets/29_edit_server_js.JPG)