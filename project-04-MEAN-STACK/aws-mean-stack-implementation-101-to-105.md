
# MERN Stack Development

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

![image](assets/4_create_security_group.JPG.jpg)

- View the status of instance created

  ![image](assets/6_view_instance.jpg)

- View Instance Details

  ![image](assets/7_view_instance_details.jpg)

- Configuring Security Group with Specific Inbound Rules
  When setting up a security group for your EC2 instance, you can control which traffic reaches your instance through inbound rules.

For SSH (port 22), this rule allows secure shell access to your instance. By default, SSH is open to any IP address (0.0.0.0/0), which is useful for testing but insecure for production. It's recommended to restrict this access to trusted IPs to reduce exposure to unauthorized login attempts.

For HTTP (port 80), this rule allows web traffic from anywhere on the internet. This is essential if you're hosting a public-facing website or web service that users can access over HTTP.

For HTTPS (port 443), this rule enables secure web traffic. Like HTTP, HTTPS traffic is generally allowed from anywhere on the internet, ensuring encrypted access for your users. This is crucial for secure communication, especially for websites dealing with sensitive data.

By configuring these rules, your instance will allow SSH access for management and handle both HTTP and HTTPS traffic, making it accessible to the public while still maintaining necessary security measures.

  ![image](assets/8_create_inbound_rules.jpg)
- Connect to instance from ssh client

   ![image](assets/9_connect_instance.jpg)
**Give Permission for the Private SSH Key**
  
  This command ensures that this  private SSH key has the correct permissions before using it to connect to your instance.

  ```
  chmod 400 "gashaw_key.pem"
  ```
  ![image](assets/10_give_permision.jpg)
**Connecting to the Instance via SSH**

Once the private key file has the correct permissions, you can use SSH to connect to your EC2 instance using its public IP address or domain name.
```
ssh -i "gashaw_key.pem" ubuntu@ec2-54-226-142-99.compute-1.amazonaws.com
```
![image](assets/11_connect_to_ssh_client.jpg)
