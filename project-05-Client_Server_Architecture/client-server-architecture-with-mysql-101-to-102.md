
# Client-Server Architecture with MySQL
# Understanding Client-Server Architecture

**Client-Server** refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.

In their communication, each machine has its own role: the machine `sending` requests is usually referred as *Client* and the machine responding (serving) is called *Server*.

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/eda684be-8aea-4a72-a3da-6c712461c897)


In the client-server architecture with MYSQL, our Web Server has a role of a `Client` that connects and reads/writes to/from a Database (DB) Server (MySQL, MongoDB, Oracle, SQL Server or any other), and the communication between them happens over a Local Network (it can also be Internet connection, but it is a common practice to place Web Server and DB Server close to each other in local network).

# TASK - Implement a Client Server Architecture using MySQL Database Management System (DBMS).

To demonstrate a basic client-server using MySQL RDBMS, follow the below instructions

1. Create and configure two Linux-based virtual servers (EC2 instances in AWS)

- **Server A** name  `mysql server`

- **Server B** name - `mysql client`

**Let us see how to create mysql server and follow same steps too create mysql client server**
1. Log to aws account console and create EC2 instance of t2.micro type with Ubuntu 24.04 LTS (HVM) Server launch in the default region us-east-1. name instance mysql server  
   
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/da219d84-f1c2-427d-b997-95f56c790410)

2. Application and OS Images select Ubuntu free tire eligable version

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/353f11ca-78b4-4fd8-8d01-b818cb79201b)


3. Create new key pair or select existing key

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/84691a41-99c6-48c0-91bc-bd08bb94d94b)

4. Network setting create new security group or use existing security group

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d14a769b-0d06-4540-a1fb-6cebe1e81704)

5. Configure Storage and launch the instance

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0fb930d1-1cd0-4a15-80b5-8a24c6319433)

6. View Instance

   ![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/40d89a99-8fa6-4862-8ee0-8a5dc9ac2ff3)

7 . Instance Details for MYSQL server

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/2116f627-15eb-4b08-8356-84806f1c4d42)

8. Configure security group with the following inbound rules:
   
- Allow traffic on port 22 (SSH) with source from any IP address.
- Allow traffic on port 3306  with source from mysql client IP address 172.31.16.13 . 

  ![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/19cadc41-0a40-4572-92d6-3c0dde88d279)

### For **Server B** name - `mysql client`follow same steps and our final instance detail looks like this 

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/9e483b90-3545-408e-833c-9d46f03e2df0)


![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a1e6ced5-eda2-424f-a4e1-ca047f706211)


**Now Two Servers UP are we can go to do our client server architecture**

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/96dd95db-9ca3-4a94-aa04-fbd4f6c09c58)


3. On **mysql server Linux Server(Server A)** install MySQL Server software.
> Interesting fact: MySQL is an open-source relational database management system. Its name is a combination of `My`, the name of co-founder Michael Widenius's daughter, and `SQL`, the abbreviation for Structured Query Language.


  1.1.  Run sudo apt update -y for latest updates on server
  
  ``` 
  sudo apt update -y
  ```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e031bdd2-c569-4894-963f-eb48abbfc837)

    
  1.2.  Install MySQL Server software
    
  ```
  sudo apt install mysql-server
  ```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7dde3efd-fedd-4203-b430-53940596a63c)


1.3 Start server

```
sudo systemctl enable mysql
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/128789e5-ea72-46e2-abdd-e3293d200266)

1.4.  Check the status to ensure it is running

```
sudo systemctl status mysql
```   
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0220a082-6d7d-47be-affb-1171931313dd)
 
3. On mysql client Linux Server install MySQL Client software.

1.1.  Run sudo apt update -y for latest updates on server
 
  ``` 
  sudo apt update -y
  ```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/faa8696a-ce05-4d83-a147-b92dc55b655b)

    
1.2.  Install MySQL Client software
    
  ```
  sudo apt install mysql-client
  ```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d3c39433-26e1-4e2d-b9ea-e1d55fa52371)

   
4. By default, both of our EC2 virtual servers are located in the same local virtual network. So they can communicate to each other using local IP addresses.

 Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default so  we will have to open it by creating a new `Inbound rules` in `mysql server` Security Groups.

> For extra security, do not allow all IP addresses to reach your `mysql server` - allow access only to the specific local IP address of your `mysql client`.

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/59c56561-2b1e-49f8-abed-1f1682e8668c)

5. You might need to configure **MySQL server** to allow connections from remote hosts and replace the following line:bind-address = `127.0.0.1`  with the following line bind-address = `0.0.0.0`
   
```
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/47c3bf67-cbae-4ec3-861a-1b61d476b331)

`
place '127.0.0.1' to '0.0.0.0' like this:
`
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d670cb70-9d2d-4540-8863-f9ec0edd130a)

**Creating a Dedicated MySQL User and Granting Privileges**

```
CREATE USER 'melkamu'@'172.31.16.13' IDENTIFIED BY 'PassWord.1';
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/da1a128b-08bf-4d48-9b8d-0ce91e79d7af)

```
GRANT ALL PRIVILEGES ON * . * TO 'melkamu'@'172.31.16.13';
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/390f3a25-ee66-4cab-94c0-23422f948bbd)

**Exit MySQL and restart the MySQL service using**
```
sudo systemctl restart mysql
```
> **Note**: '172.31.16.13' is the ip address of your mysql-client server

6. From aysql client Linux Server connect remotely to mysql server Database Engine without using ssh. You must use the mysql utility to perform this action.
```
mysql -u melkamu -h 100.26.107.253 -p
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a4092086-3225-446d-8b09-05eb4cf79f96)

8. Check that you have successfully connected to a remote MySQL server and can perform SQL queries:
   
```
Show databases;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/56097bff-2919-47ff-819e-9ca494bb46e8)


## The End of Client-Server Architecture using MySQL project
In this Project We have used  One of the most widely used server-side technologies for data storage is MySQL, an open-source relational database management system.
 we see  a step-by-step guide on how to implement client-server architecture with MySQL.
