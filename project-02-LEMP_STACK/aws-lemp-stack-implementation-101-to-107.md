LEMP is an open-source web application stack used to develop web applications. The term LEMP is an acronym that represents **L** for the Linux Operating system, Nginx (pronounced as engine-x, hence the **E** in the acronym) web server, **M** for MySQL database, and **P** for PHP scripting language.
## The LEMP stack is a combination of four open-source technologies that are used in web development. These technologies include:

- **Linux:** The operating system that runs the web server.
- **Nginx:** The web server software that handles HTTP requests.
- **MySQL:** The relational database management system that stores the website’s data.
- **PHP:** The programming language used to build dynamic web applications.

## Project 2: LEMP STACK Web Development
Project 2 covers similar concepts as Project 1 and help to cement your skill of deploying Web solutions using LA(E)MP stacks

**1. Launch instance in AWS Console**
The first step in implementing this project is launch an instance in AWS console

create an instance in the default region us-east-1 and enter instance name **LEMP STACK Web Server**

![image](assets//1_launch_instance_name.jpg)
- Next we select ubuntu operating system for an instance 
![image](assets/2_select_ubuntu_instance.jpg)

- Create or use existing  private key to log into the instance created
![image](assets/3_key_pair_new_existing.jpg)

- Choose the **volume size** and **type** for the instance created
![image](assets/5_configure_and_launch_instance.jpg)
- Configuring the security group in AWS EC2
  
A security group in AWS acts as a virtual firewall for your EC2 instances. It controls both inbound and outbound traffic to ensure only the permitted traffic reaches your instance. Each EC2 instance must be associated with at least one security group. The security group rules can be customized to define the type of traffic that is allowed to connect to the instance, including protocols, ports, and IP addresses.
![image](assets/4_create_security_group.jpg)
