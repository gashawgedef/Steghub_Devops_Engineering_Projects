# DevOps Tooling Website Solution
> This project focuses on building a website solution for a team of developers that will help a in day to day activities in managing, developing, testing, deploying, and monitoring different projects
### In this project, we will implement a solution that consists of the following components:

1. Infrastructure: AWS
2. Webserver Linux: Red Hat Enterprise Linux 8
3. Database Server: Ubuntu 24.04 + MySQL
4. Storage Server: Red Hat Enterprise Linux 8 + NFS Server
5. Programming Language: PHP
6. GitHub Code Repository.

### Prerequisites

1. Knowledge of AWS core services and CLI
2. Basic knowledge of Linux commands and how to manage storage on a Linux server.
3. Basic knowledge of Network-attached storage (NAS), Storage Area Networks (SAN), and related protocols like NFS, FPT, SFTP, SMB, iSCSI.
4. Knowledge of Block-level storage and how it is used on the Cloud.
5. 
#### ARCHTECTURAL DESIGN

![image](assets/images1.png)

## Step 1 - Prepare NFS Server
1. Spin up a new EC2 instance with RHEL Linux Operating System.
   
**Log to aws account console and create EC2 instance of t2.micro type with RedHat Server launch in the default region us-east-1. name instance _Linux NFS server_**
![image](assets/nfs_server_1_instance_name.JPG)

**Application and OS Images select RedHat free tire eligable version**
![image](assets/nfs_server_2_select_os.JPG)

**Create new key pair or select existing key**
![image](assets/nfs_server_3_key_pair.JPG)

**Network setting create new security group or use existing security group**
![image](assets/nfs_server_4_security_gruop.JPG)

**Configure Storage and launch the instance**
![image](assets/nfs_server_5_configure_storage.JPG)

**View Instance**
![image](assets/nfs_Server_6_view_instance.JPG)

**Instance Details for web**
![image](assets/nfs_server_7_view_details.JPG)


**Configure security group with the following inbound rules:**
- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default.
- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.
  ![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/83aa77a2-0445-45fa-b968-0fad36af7d96)

2. Based on your LVM experience from Project 6, Configure LVM on the Server. Create 3 volumes in the same AZ as your Web Server EC2, each of 20 GiB.

**Add EBS Volume to an EC2 instance**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/86d4d944-ce76-49e3-9b0e-682e330ed485)

**Attach all three volumes one by one to our NFS Web Server EC2 instance**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/64c26f72-13aa-487d-a9f0-c40365672dc5)

 **Open up the Linux terminal to begin configuration**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/84be9f7d-7685-4843-9a2b-02a09e95bf97)

  **List Available Disks**
```
sudo lsblk
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b9686c5d-0179-40e0-b444-8974460f0b35)

**Use df -h command to see all mounts and free space**
```
df -h
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5e0c9ecc-4988-4742-985b-7272a26f3f9e)

**Use gdisk utility to create a single partition on each of the 3 disks**
```
sudo gdisk /dev/xvdb
```
**List Existing Partitions: To see the current partitions, use the `p` command:**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/2004ce38-2dd7-4dd0-b1d2-06ecd52e5809)

**Create a New Partition: To add a new partition, enter `n`: then Press Enter to accept default value**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/db6f43d8-ee17-4cb1-bb9d-a71cbc92d928)

**Write Changes: Once you've created the desired partitions, write the changes to the disk with `w`:**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6b187aed-4386-4494-b256-e8a12dff6ae8)

**Yes to proceed and complete**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/fddb01f3-626f-429f-9036-6ea5022be36e)

> we follow the same steps for remaining two and create partishion

**Use lsblk utility to view the newly configured partition on each of the 3 disks.**
```
lsblk
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/55299708-5cfa-4abd-ae9a-7234303b2b6a)

**Install lvm2 package**
```
sudo yum install lvm2
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/55b48752-0cc7-4a5a-971a-ea0bfa8744ea)

**Check for available partitions.**

```
sudo lvmdiskscan 
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/056282fe-8f59-4615-a393-2e0a1f469e95)

**Create Physical Volumes Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM**
```
sudo pvcreate /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e8165611-f10c-4838-9fcb-2790fb1e3fa1)

**Verify that your Physical volume has been created successfully**
```
sudo pvs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e8ead4b9-48fc-44e1-b57e-c1c6ad3bd2a3)

**Use vgcreate utility to add all 3 PVs to a volume group (VG) Name the VG webdata-vg**
```
sudo vgcreate webdata-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/dd7feb65-047e-4189-854b-418e5fef1250)

**Verify that your VG has been created successfully**
```
sudo vgs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/761cec16-28e5-426a-aa1e-f579e142da4f)

**Create Logical Volumes Use `lvcreate utility` to create logical volumes
```
sudo lvcreate -L 14G -n lv-apps webdata-vg
sudo lvcreate -L 14G -n lv-logs webdata-vg
sudo lvcreate -L 14G -n lv-opt  webdata-vg
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6b6b58ef-e55a-4a70-be2e-7a4abad0789a)

**Verify that our Logical Volume has been created successfully**
```
sudo lvs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d0510c5e-f675-4650-8165-b08e8f585bfc)

**Verify the entire setup #view complete setup - VG , PV, and LV**
```
sudo vgdisplay -v
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8ef02e1e-28c6-4ff9-aa8c-866a1e473451)

3. Instead of formatting the disks as ext4 you will have to format them as xfs
- Ensure there are 3 Logical Volumes `lv-opt` `lv-apps`, and `lv-logs`
```
sudo lsblk
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/30185aa4-061f-49e7-8f37-0e6f53b09619)

**Format the Logical Volumes as XFS:**
```
sudo mkfs.xfs /dev/webdata-vg/lv-apps
sudo mkfs.xfs /dev/webdata-vg/lv-logs
sudo mkfs.xfs /dev/webdata-vg/lv-opt
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/66b58d40-98f6-43d3-96a0-8e696f21b1b2)

- Create mount points on `/mnt` directory for the logical volumes as follows: `Mount lv-apps` on /mnt/apps - To be used by webservers ,`Mount lv-logs` on /mnt/logs - To be used by webserver logs, `Mount lv-opt` on /mnt/opt - To be used by Jenkins server in Project 8

**Create Directories**:
```
sudo mkdir /mnt/apps
sudo mkdir /mnt/logs
sudo mkdir /mnt/opt
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d967f7e3-0d89-4e68-9dea-adf3014d771d)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/35a8afb6-96c6-487c-88a0-f2a6ca9fb4c7)

**Mount Logical Volumes**
```
sudo mount /dev/webdata-vg/lv-apps /mnt/apps
sudo mount /dev/webdata-vg/lv-logs /mnt/logs
sudo mount /dev/webdata-vg/lv-opt /mnt/opt

```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/f4308135-587f-4c6d-919b-0b61b61da04c)

**Add Mount Points to /etc/fstab**
```
sudo vi /etc/fstab
```
**Add the following lines**:
```
/dev/webdata-vg/lv-apps /mnt/apps xfs defaults 0 0
/dev/webdata-vg/lv-logs /mnt/logs xfs defaults 0 0
/dev/webdata-vg/lv-opt /mnt/opt xfs defaults 0 0
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/91a056b5-d84f-45bb-87e3-ab83ac6597d8)

**Verify Mounts**:

```
sudo mount -a
```
```
df -h
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/4662fc86-dc51-4a26-88d3-f89fcbcd7ec6)

4. Install NFS server, configure it to start on reboot and make sure it is up and running

**Update the System and Install NFS Utilities**:
```
sudo yum -y update
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8d37fffe-f3a2-4a64-8627-44f348c045e3)

```
sudo yum install nfs-utils -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/29c33604-b690-49e1-a027-2e2d917cc0d7)

**Start and Enable NFS Server**:

```
sudo systemctl start nfs-server.service
```
```
sudo systemctl enable nfs-server.service
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0c99fba5-5916-4a08-a1a4-5b4d340c57bc)

```
sudo systemctl status nfs-server.service
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/048af360-e331-41c5-a7a9-eaf2e29949ba)


5. Export the NFS Mounts'

> Use `subnet cidr` to connect as clients. For simplicity, we will install our all three Web Servers inside the same subnet, but in _production_ set up  would probably want to separate each tier inside its own subnet for higher level of security. To check our subnet cidr - open our EC2 details in AWS web console and locate `Networking` tab and open a Subnet link:

**Set Permissions on Mount Points**
```
sudo chown -R nobody:nobody /mnt/apps
sudo chown -R nobody:nobody /mnt/logs
sudo chown -R nobody:nobody /mnt/opt
sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/69154fdf-2a59-42df-86c9-7456789f7fc1)

**Restart NFS Server**
```
sudo systemctl restart nfs-server.service
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1bb1674a-2fb4-461c-b663-683d006c02e0)

6. Configure access to NFS for clients within the same subnet (example of Subnet CIDR - 172.31.16.0/20 ):
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b37f58e1-d9c1-451b-91aa-224b316f74d1)

```
sudo vi /etc/exports
```
**Add the following lines**:

```
/mnt/apps 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/opt 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1818c80f-790d-4cff-9fde-52e27fad870d)

**save and exit from the editor by** `Esc + :wq!`

**Export the NFS Shares**:

```
sudo exportfs -arv
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/46535253-4d4c-4de0-bc1f-98359d16e92e)


7. Check which port is used by NFS and open necessary ports in Security Groups (add new Inbound Rule)

**Check NFS Ports**:

```
rpcinfo -p | grep nfs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d95f15bf-aad7-456a-a855-75c679105aed)

> **Important note**: In order for NFS server to be accessible from our client,we open following ports:
- `TCP 111`
- `UDP 111`
- `UDP 2049`
- `UDP 2049`
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/801f96f8-47ce-4645-ad4a-aa55659e83f6)

## Step 2 - Configure the database server
Log to aws account console and create EC2 instance of t2.micro type with Ubuntu Server launch in the default region us-east-1. name instance MySQL server
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7692b0cf-2cf2-4692-a726-592ff2e2f58c)
 Follow the same step and finally the instance created like this 
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1422fff3-ba37-4b08-8919-cbeaa821813a)

1. Install MySQL Server
First, we need to connect to our  Mysql server server
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/4bac19a7-71cb-4a06-9b89-b29d4b48af48)

1.1. Update the package index:
```
sudo apt update
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6412cdbe-5c23-4e82-aedd-f599db0b6da8)

1.2. Install MySQL server:
```
sudo apt install mysql-server
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3d39ad91-22a5-463f-a601-cc5ffb731836)

1.3 Check status :
```
sudo systemctl status mysql
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/983caa69-beed-4e0e-8f13-cfd18dc42d66)

1.4. Enable MySQL to start on boot:
```
sudo systemctl enable mysql
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d8fdde26-9b5c-4a2e-8896-4d9a499053af)

1.5. Secure the MySQL installation (set root password, remove test databases, etc.):
```
sudo mysql_secure_installation
```
Follow the prompts to complete the configuration.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b9468974-933f-4248-bb02-5147812f5664)


2. Create a database and name it tooling

 2.1. Log in to the MySQL server as the root user:
```
sudo mysql -u root -p
```
2.2. Create the database:
```
CREATE DATABASE tooling;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3874798a-58c7-4356-833e-9d978b2381ee)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a1361f43-77a0-4ed3-90ee-d35855611192)

3. Create a database user and name it `webaccess`

```
CREATE USER 'webaccess'@'%' IDENTIFIED BY 'PassWord.1';
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/14b641d4-8b59-4270-bcc4-40daa0a11f93)

4. Grant permission to webaccess user on tooling database to do anything only from the webservers subnet `cidr`
> To grant permissions to a MySQL user based on a specific subnet (CIDR), you can use a series of wildcard characters to match the IP address range. MySQL does not support direct CIDR notation, so you need to translate the CIDR range into a pattern that MySQL understands.

```
GRANT ALL PRIVILEGES ON tooling. * TO 'webaccess'@'%' WITH GRANT OPTION;
```
**Apply the changes**:
```
FLUSH PRIVILEGES;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7b2c57e2-a225-415f-acb5-6541b65b07e0)

**Exit MySQL**
```
exit
```
## Step 3 - Prepare the Web Servers

 **In this step we will do the following**
 - Configure NFS client (this step must be done on all _three servers_)
 - Deploy a Tooling application to our Web Servers into a shared NFS folder
 - Configure the Web Servers to work with a single MySQL database
**For server One**

1. Launch a new EC2 instance with RHEL 8 Operating System

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8002b412-e91f-4172-abbc-c6410d6aaea1)
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5f91a1d0-1cfe-4ae5-a2c3-b5a6ee820c4e)
**Follow same steps above and  our instance detail look like this**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b69e64a4-08cb-4a0e-aefb-7ee10198ac13)


2. Install NFS client
```
sudo yum update -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/cf036f39-cd2e-44d8-9fe7-701c45710705)


```
sudo yum install nfs-utils nfs4-acl-tools -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ce01fb6d-486a-457c-a4bd-e5a68cfa0f66)

3. Mount /var/www/ and target the NFS server's export for apps
```
sudo mkdir /var/www
```
```
sudo mount -t nfs -o rw,nosuid 172.31.28.68:/mnt/apps /var/www
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b754b43d-bc0a-4e01-9e37-dfb6d9f24081)

4. Verify that NFS was mounted successfully
```
 df -h
 ```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7e32ebc5-2427-44e8-9f10-133e7ccb073d)
  
Make sure that the changes will persist on Web Server after reboot:
```
sudo vi /etc/fstab
```

add following line
```
172.31.28.68:/mnt/apps /var/www nfs defaults 0 0
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/489120b5-f8ef-4447-a8bc-1d71a97d6ce8)


5. Install Remi's repository, Apache and PHP

```
sudo yum install httpd -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/9eedcd74-6bb8-4bf7-a2ce-9ba9cca730dd)

```
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/01fae725-f4fe-49ae-85b6-f057e59eaa7d)

**To confirm that EPEL has been added**
```
 rpm -qi epel-release
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c672f3aa-c506-48da-bb6d-c39d0146c29a)

```
sudo dnf -y install http://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/10736900-cd9c-4499-9e36-f77187172f16)

> If you encounter the "Killed" message while running , it likely means the process was terminated due to running out of memory. Here are steps to mitigate this:
Updating System with Limited Memory

Create a swap file to increase virtual memory:

```
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ee6c73c8-a874-44a8-b382-8bf2cf2d5ad8)

**Make the swap file permanent:**
```
echo '/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8bd755c4-c4cc-4dac-8960-053e1a817a32)

**Update and Clean the System and re run**
```
sudo dnf upgrade --refresh -y
sudo dnf upgrade -y dnf
sudo dnf update -y
sudo dnf clean all
sudo reboot
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d1ca49c4-ab12-4389-b4c0-9ae10098882e)

**Tehn Re run again**

```
sudo dnf -y install http://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/065c3670-bd16-4d08-8c8d-7c58bb8688fb)

Before installing PHP, we need to check the available PHP streams in the repository.
```
 sudo dnf module list php -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/cdae6be5-ba91-4c66-8e0c-aa9b4c245527)

```
sudo dnf module reset php
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/9804b9e9-7232-400d-a9c9-19bd5cc8ad9b)


```
sudo dnf module enable php:remi-8.2 -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/f8bdb6dc-929c-46b4-8d26-7b742640ec72)

```
sudo dnf install php -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ad7c8f5c-d595-450d-8e97-61dc2140fbd0)

```
php -v
```

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/99fe167a-03da-4f7b-99fc-84007726a6c2)

```
sudo systemctl start php-fpm
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/4420ac86-6cf0-4271-b620-47bea2687ccb)


```
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b9281e76-5e0a-4066-8ad1-bc7f33f64f1a)


```
sudo systemctl enable php-fpm
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/be84c348-be23-4d87-9bf4-954f58b8c19c)

```
sudo setsebool -P httpd_execmem 1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/fec48723-2c5b-4e90-a761-f01947b43956)

> Repeat steps 1-5 for another 2 Web Servers and finally 
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/cc6d6668-a90f-45a8-8d17-1bbf276e1997)





6. Verify NFS Mount and Apache Setup 

Verify that Apache files and directories are available on the Web Server in /var/www and also on the NFS server in /mnt/apps. If you see the same files - it means NFS is mounted correctly.

```
cd /var/www
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3520ec68-988d-4e7d-9dcc-5b4556eb5ee4)

```
ls -l
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b7349964-4951-47cb-ae55-49b3ba5db997)

 You can try to create a new file
```
sudo touch test.txt
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/421dcfe1-c487-4117-922f-e09b9400ca17)


![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0ba0d04f-8d4b-4b77-a78f-6243ef3d526b)

**We can see the text.txt file created inside our nfs server /mnt/apps directory. So they are communicating perfectly.**

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1111c017-196b-45c6-a1e6-9f684377beb8)

from one server and check if the same file is accessible from other Web Servers.

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/be74d92a-90d5-4cd5-888d-a09b0dcd9947)


7. Locate the log folder for Apache on the Web Server and mount it to NFS server's export for logs. Repeat step №4 to make sure the mount point will persist after reboot.
```
sudo mkdir -p /var/log/httpd
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/24b30052-2f58-4ae1-b8bf-a8597684cd29)

```
sudo mount -t nfs -o rw,nosuid 172.31.28.68:/mnt/logs /var/log/httpd
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b6a50485-dd65-461f-b923-abdfa7a7d79e)

Edit the /etc/fstab file so that it persists even after reboot
```
sudo vi /etc/fstab
```
add the following lines
```
172.31.28.68:/mnt/logs /var/log/httpd nfs defaults 0 0
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0658920b-fad5-4e76-b08f-614260e16c89)

8. Fork the tooling source code from Darey.io Github Account to your Github account.
**Download git**
```
sudo yum install git
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1f1f26f8-da14-4f1d-a75f-d361d36dfe7d)

**Clone the repository you forked the project into**
```
git clone https://github.com/melkamu372/tooling.git
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/cd9e36f0-7129-4029-9808-15d4177c54d0)

9. Deploy the tooling website's code to the Webserver. Ensure that the html folder from the repository is deployed to /var/www/html
```
sudo cp -R html/. /var/www/htm/
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/004aaed6-4085-46f4-92fa-e0dfccce8d52)

> Note 1: Do not forget to open TCP port 80 on the Web Server.

> Note 2: If you encounter 403 Error - check permissions to your /var/www/html folder and also disable SELinux sudo setenforce 0 To make this change permanent - open following config file

**Disable SELinux**
```
sudo setenforce 0
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0519607c-34bb-4247-b8e2-68816857610e)

```
sudo vi /etc/sysconfig/selinux
```
and set SELINUX=disabled

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b3b63124-cee0-41b6-b5f6-186b6323f0b8)

 **Then restrt httpd**.
```
sudo systemctl restart httpd
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1c12dbb9-3222-4a82-aeee-aa74c793ca5b)
```
sudo systemctl status httpd
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3c262a40-9026-48a8-9c85-3e9c5c511cbf)


10. Update the website's configuration to connect to the database (in /var/www/html/functions.php file). Apply `tooling-db.sql` script to your database using this command
 
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3b1d214e-7092-4593-838d-e695f824f671)

 ```
  mysql -h 172.31.21.131 -u webaccess -p -D tooling < tooling-db.sql

```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e022be24-ef1a-44a3-b2ea-b8115e1d7ce0)


11. Create in MySQL a new admin user with `username: myuser` and `password: password`:

```
mysql -h 172.31.21.131 -u webaccess -p
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/4ab2076d-1cba-4629-b11b-4ae9c5ce5a69)

```
INSERT INTO 'users' ('id', 'username', 'password', 'email', 'user_type', 'status') VALUES -> (2, 'webaccess_user', '5f4dcc3b5aa765d61d8327deb882cf99', 'user@mail.com', 'admin', '1');
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/340ba5b3-0b3b-4471-8f2a-a9deb1946ef1)

12. Open the website in your browser http://`Web-Server-Public-IP-Address`or `Public-DNS-Name`/index.php and make sure you can login into the website with  your user and password.

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/edf887e7-e267-43eb-9996-85d46b239605)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/098e7ff2-693b-4ae1-a247-623d1e9a80d7)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/300c579c-92d6-430d-81d1-34b2d362e82b)

### End of the Project
We successfully implemented and deployed a DevOps tooling website, providing easy access to DevOps tools within the corporate infrastructure. The solution includes multiple web servers sharing a common database and accessing the same files using Network File System (NFS) for shared storage.

