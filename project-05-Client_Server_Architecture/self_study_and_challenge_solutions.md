# Self Study
# 1. Read about ping and traceroute network diagnostic utilities. Be able to make sense out of the results of using these tools.

## Ping and Traceroute: Network Diagnostic Utilities

Ping and traceroute are two essential tools for network troubleshooting. They work together to diagnose different aspects of connectivity issues:
### Ping

**Ping** is a network utility used to test the reachability of a host on an IP network and measure the round-trip time for messages sent from the originating host to a destination computer. It uses Internet Control Message Protocol (ICMP) Echo Request messages to elicit an ICMP Echo Reply from the target host.



**How Ping Works:**

- Ping sends **ICMP (Internet Control Message Protocol)** Echo Request packets to the target device.

- The target replies with an Echo Reply packet.

- It measures the **round-trip time (RTT)** it takes for packets to go from the source to the destination and back.

When using Ping, always test a few different sites to see if it is just one site or all sites.

![image](assets/self_ping.JPG)

You can interrupt Ping at any time by holding down the CTRL key, and pressing C on your keyboard.

**Interpreting Ping Results:**
- **Reply from [IP Address]:** Indicates the host is reachable.
- **Request timed out:** No response was received, possibly due to network issues or the host being down.
- **Packets sent/received/lost:** Helps determine packet loss.
- **RTT (ms):** Shows minimum, maximum, and average round-trip times, indicating latency.


Ping operates by sending ICMP Echo Request packets to the target device and waiting for an ICMP Echo Reply. The program reports errors, packet loss, and a statistical summary of the results.
### Example: target device not responding
A ping result, where the target device is not responding, or there is a connection issue, will look like this

![image](assets/self_pint_reachtimeout.JPG)


Although four packets were sent, none have been received, showing a 100% loss of packets, and indicating an issue with either the connection or the target device.

> **Note**: A result like this does not always mean the device is not online or working correctly. Many devices have ICMP ping responses disabled for security or service reasons. So even if they are up and running it will appear as if they are unreachable.


### Traceroute

**Traceroute** is a network diagnostic tool used to track the pathway that a packet takes from the source to the destination. It records the route (the specific gateway computers at each hop) through the Internet between your computer and a specified destination computer. It also calculates and displays the time taken for each hop.

**How Traceroute Works:**
1. **ICMP/TCP/UDP Probes:** Traceroute sends a sequence of packets with incrementally increasing Time-To-Live (TTL) values.
2. **TTL Exceeded:** Each router that forwards the packet decreases the TTL by one. When the TTL reaches zero, the router discards the packet and sends an ICMP "Time Exceeded" message back to the source.
3. **Tracing the Path:** By recording the source of each "Time Exceeded" message, Traceroute determines the path taken by the packets to the destination.
```
tracert google.com
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/744f030f-7d62-4739-a023-0182217550dc)

> **Note**: You can interrupt Traceroute at any time by holding down the CTRL key, and pressing C on your keyboard

**Interpreting Traceroute Results:**
- **List of Hops:** Each line represents a hop from the source to the destination.
- **IP Address and Hostnames:** Shows the IP address and, if resolvable, the hostname of each hop.
- **RTT for Each Hop:** Provides round-trip times for each hop, often three times per hop.

**Key Points:**
- **Hop Count:** The number of hops (intermediate routers) to reach the destination.
- **Latency per Hop:** Helps identify where delays are occurring in the path.
- **Unreachable Nodes:** If a hop consistently fails to respond, it might indicate a network problem at that hop.

> Understanding how to use and interpret the results from Ping and Traceroute,help diagnose and troubleshoot network issues effectively.

### Using PathPing

**PathPing** is a network utility that combines the functions of `ping` and `tracert` to provide detailed information about the path between your computer and a destination. It sends packets to each router on the way to a final destination and reports the results over a period of time, showing the degree of packet loss at each router or link.

### How to Use PathPing

1. **Open Command Prompt**:
   - Press `Win + R`, type `cmd`, and press `Enter`.

2. **Run PathPing Command**:
   - Type `pathping <destination>` and press `Enter`.
   - `<destination>` can be an IP address or a domain name (e.g., `pathping google.com`).

3. **Interpret the Results**:
   - The first part of the report shows the path and round-trip times to each hop.
   - The second part shows the packet loss and latency at each hop after sending packets for a certain duration.

### Understanding MTR (MyTraceRoute)

**MTR** is a network diagnostic tool that combines the functionality of `ping` and `traceroute`. It continuously sends packets and updates the results in real-time, making it easier to diagnose network issues.

### How to Use MTR

1. **Install MTR**:
   - On **Linux**: You can install MTR using a package manager.
     ```sh
     sudo apt-get install mtr  # For Debian/Ubuntu
     ```
     ```
     sudo yum install mtr  # For CentOS/RedHat
     ```
   - On **macOS**: Install MTR using Homebrew.
     ```sh
     brew install mtr
     ```
   - On **Windows**: Download and install the Windows version of MTR from [MTR's official website](https://github.com/traviscross/mtr).

2. **Run MTR Command**:
   - Open your terminal or command prompt.
   - Type `mtr <destination>` and press `Enter`.
   - `<destination>` can be an IP address or a domain name (e.g., `mtr google.com`).

3. **Interpret the Results**:
   - The results show each hop on the route to the destination.
   - Columns include:
     - **Loss%**: Packet loss percentage at each hop.
     - **Snt**: Number of packets sent.
     - **Last**: Latency of the last packet.
     - **Avg**: Average latency.
     - **Best**: Best (minimum) latency.
     - **Wrst**: Worst (maximum) latency.
     - **StDev**: Standard deviation of the latency.

### Key Differences and Use Cases PathPing & MTR

- **PathPing**: Ideal for a detailed, single-time diagnostic of packet loss and latency over time.
- **MTR**: Suitable for real-time, continuous monitoring of network paths, making it easier to spot transient issues.

Both tools are powerful for diagnosing network issues, helping identify problematic hops and potential sources of latency or packet loss.


# 2. Refresh your knowledge of basic SQL commands, be able to perform simple SHOW, CREATE, DROP, SELECT and INSERT SQL queries.
SQL stands for Structured Query Language. SQL commands are the instructions used to communicate with a database to perform tasks, functions, and queries with data.

SQL commands can be used to search the database and to do other functions like creating tables, adding data to tables, modifying data, and dropping tables.

Here is a list of basic SQL commands (sometimes called clauses) you should know if you are going to work with SQL.
# Basic SQL Commands

## SHOW

The `SHOW` command is used to display database objects and their properties. It can show tables, databases, columns, and more.

**Examples:**
- **Show all databases:**
```
SHOW DATABASES;
```
- **Show all tables in the current database:**
```
SHOW TABLES;
```
- **Show columns of a specific table**:

```
SHOW COLUMNS FROM table_name;
```
### CREATE
The CREATE command is used to create databases, tables, views, and other database objects.

**Examples**:

- **Create a new database**:
```
CREATE DATABASE database_name;
```
- **Create a new table**:
```
CREATE TABLE table_name (
    column1_name data_type constraints,
    column2_name data_type constraints,
    ...
);
```
**Example**:
```
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    position VARCHAR(50),
    salary DECIMAL(10, 2)
);
```
### DROP
The DROP command is used to delete databases, tables, or other database objects.

**Examples**:

- **Drop a database**:
```
DROP DATABASE database_name;
```
- **Drop a table**:
```
DROP TABLE table_name;
```

### SELECT
The SELECT command is used to query data from one or more tables.

**Examples**:

- *Select all columns from a table:*
```
SELECT * FROM table_name;
```
- *Select specific columns from a table*:
```
SELECT column1_name, column2_name FROM table_name;
```

- *Select with a condition*:
```
SELECT * FROM table_name WHERE condition;
```
Example:
```
SELECT name, position FROM employees WHERE salary > 50000;
```
### INSERT
The INSERT command is used to add new rows of data into a table.

**Examples**:

- Insert a single row:
```
INSERT INTO table_name (column1_name, column2_name, ...)
VALUES (value1, value2, ...);
```
**Example**:
```
INSERT INTO employees (name, position, salary)
VALUES ('John Doe', 'Manager', 75000);
```
- Insert multiple rows:

```
INSERT INTO table_name (column1_name, column2_name, ...)
VALUES 
(value1, value2, ...),
(value3, value4, ...),
...;
```

**Example**:
```
INSERT INTO employees (name, position, salary)
VALUES 
('Jane Smith', 'Developer', 60000),
('Mike Johnson', 'Analyst', 55000);
```

# Practical example how to use basic SQL commands

**Let's combine these commands in a practical scenario**:
0. Log into your host and next to  MySQL/MariaDB
```
mysql -u root -p
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6c44a14a-0601-4c3c-8b57-769e6365094c)


1. Show existing databases:

```
SHOW DATABASES;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a9243981-bacc-463a-8a22-62261e308110)

2.  Create a new database:
```
CREATE DATABASE company;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/48faae71-3a72-43b4-97b5-ee9439d85871)

3. Switch to the new database:
```
USE company;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/4a1b2a7a-5371-4347-bbf1-2a8bbee7ad99)

4. Create a new table:
```
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    position VARCHAR(50),
    salary DECIMAL(10, 2)
);
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/bc6c3f5c-0d1e-4c7f-ade5-7b0ac136a625)

5. Insert data into the table:
```
INSERT INTO employees (name, position, salary)
VALUES ('Melkamu Tessema', 'CEO', 120000);
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/583f7e0f-3286-4520-975d-9c2c962090a1)

6. Query the table:
```
SELECT * FROM employees;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a1463a86-4a91-4689-b6af-c6c2aaffcca3)

7. Drop the table:
  ```
  DROP TABLE employees
  ```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/2f190cd2-f90b-498f-8f5d-ee7bc6055f46)

## References
1. [Cloud Direct Using Traceroute, Ping, MTR, and PathPing](https://www.clouddirect.net/knowledge-base/KB0011455/using-traceroute-ping-mtr-and-pathping)
2. [Cisco Understand the Ping and Traceroute Commands](https://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-software-releases-121-mainline/12778-ping-traceroute.html)
3. [Basic SQL Commands - The List of Database Queries and Statements You Should Know](https://www.freecodecamp.org/news/basic-sql-commands/)
