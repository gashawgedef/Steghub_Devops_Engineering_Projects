# DevOps Tooling Website Solution Project 7

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

2. Based on your LVM experience from Project 6, Configure LVM on the Server. Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

**Add EBS Volume to an EC2 instance**

![image](assets/nfs_server_9_create_three_volumes.JPG)

**Attach all three volumes one by one to our NFS Web Server EC2 instance**

![image](assets/nfs_server_12_atach_3_volumes.JPG)

 **Open up the Linux terminal to begin configuration**

 ![images](assets/nfs_Server_13_give_permisions.JPG)

![image](assets/nfs_server_14_connect_server.JPG)

  **List Available Disks**
```
sudo lsblk
```
![image](assets/nfs_server_15_list_available_disks.JPG)

**Use df -h command to see all mounts and free space**
```
df -h
```
![image](assets/nfs_server_16_mount_used_and_free_space.JPG)

**Use gdisk utility to create a single partition on each of the 3 disks**
```
sudo gdisk /dev/xvdb
```
**List Existing Partitions: To see the current partitions, use the `p` command:**
![image](assets/nfs_Server_17_creat_partitions_1.JPG)

**Create a New Partition: To add a new partition, enter `n`: then Press Enter to accept default value**
![image](assets/nfs_server_18_create_partions_2.JPG)

**Write Changes: Once you've created the desired partitions, write the changes to the disk with `w`:**
![image](assets/nfs_Server_19_create_partitions_3.JPG)

**Yes to proceed and complete**
![image](assets/nfs_Server_19_create_partitions_3.JPG)

> we follow the same steps for remaining `two` and create partition

**Use lsblk utility to view the newly configured partition on each of the 3 disks.**
```
lsblk
```
![image](assets/nfs_server_20_view_partitions.JPG)

**Install lvm2 package**
```
sudo yum install lvm2
```
![image](assets/nfs_server_21_install_lvm2.JPG)

**Check for available partitions.**

```
sudo lvmdiskscan 
```
![image](assets/nfs_server_22_check_for_available_disks.JPG)

**Create Physical Volumes Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM**
```
sudo pvcreate /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
![image](assets/nfs_server_23_create_physical_volumes.JPG)

**Verify that your Physical volume has been created successfully**
```
sudo pvs
```
![image](assets/nfs_server_24_view_lvs.JPG)

**Use `vgcreate` utility to add all 3 PVs to a volume group (VG) Name the VG `webdata-vg`**
```
sudo vgcreate webdata-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
![image](assets/nfs_server_25_add_to_volume_group.JPG)

**Verify that your VG has been created successfully**
```
sudo vgs
```
![image](assets/nfs_server_26_verify_volume.JPG)

**Create Logical Volumes Use `lvcreate utility` to create logical volumes

```
sudo lvcreate -L 14G -n lv-apps webdata-vg
sudo lvcreate -L 14G -n lv-logs webdata-vg
sudo lvcreate -L 14G -n lv-opt  webdata-vg
```

![image](assets/nfs_server_27_create.JPG)

there is no enough space to create `lv-opt` and we have to make free some spaces

![images](assets/nfs_server_28_create_with_minimum_space.JPG)

**Verify that our Logical Volume has been created successfully**
``` 
sudo lvs
```
![image](assets/nfs_server_30_verify.JPG)

**Verify the entire setup #view complete setup - VG , PV, and LV**
```
sudo vgdisplay -v
```
![image](assets/nfs_server_31_display_all.JPG)

3. Instead of formatting the disks as `ext4` you will have to format them as `xfs`
- Ensure there are 3 Logical Volumes `lv-opt` `lv-apps`, and `lv-logs`
```
sudo lsblk
```
![image](assets/nfs_server_32_lsblk.JPG)

**Format the Logical Volumes as `XFS`:**
```
sudo mkfs.xfs /dev/webdata-vg/lv-apps
sudo mkfs.xfs /dev/webdata-vg/lv-logs
sudo mkfs.xfs /dev/webdata-vg/lv-opt
```
![image](assets/nfs_server_33_format_xfs.JPG)

- Create mount points on `/mnt` directory for the logical volumes as follows: `Mount lv-apps` on `/mnt/apps` - To be used by webservers ,`Mount lv-logs` on `/mnt/logs` - To be used by webserver logs, `Mount lv-opt` on `/mnt/opt` - To be used by Jenkins server in Project 8

**Create Directories**:
```
sudo mkdir /mnt/apps
sudo mkdir /mnt/logs
sudo mkdir /mnt/opt
```
![image](assets/nfs_server_34_create_directories.JPG)

**Mount Logical Volumes**

```
sudo mount /dev/webdata-vg/lv-apps /mnt/apps
sudo mount /dev/webdata-vg/lv-logs /mnt/logs
sudo mount /dev/webdata-vg/lv-opt /mnt/opt
```

![image](assets/nfs_Server_35_mount_all.JPG)

**Add Mount Points to /etc/fstab**

```
sudo vim /etc/fstab
```
**Add the following lines**:
```
/dev/webdata-vg/lv-apps /mnt/apps xfs defaults 0 0
/dev/webdata-vg/lv-logs /mnt/logs xfs defaults 0 0
/dev/webdata-vg/lv-opt /mnt/opt xfs defaults 0 0
```
![image](assets/nfs_server_36_edit_fstab.JPG)

**Verify Mounts**:

```
sudo mount -a
```
```
df -h
```
![image](assets/nfs_server_37_dfh.JPG)

4. Install NFS server, configure it to start on reboot and make sure it is up and running

**Update the System and Install NFS Utilities**:
```
sudo yum -y update
```
![image](assets/nfs_server_38_update_server.JPG)

```
sudo yum install nfs-utils -y
```
![image](assets/nfs_server_39_install_nfs_service.JPG)

**Start and Enable NFS Server**:

```
sudo systemctl start nfs-server.service
```
![images](assets/nfs_server_40_start_nfs_server_ser%20vice.JPG)

```
sudo systemctl enable nfs-server.service
```

![image](assets/nfs_server_41_enable.JPG)

```
sudo systemctl status nfs-server.service
```

![image](assets/nfs_server_42_status.JPG)


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

![image](assets/nfs_server_%2043_set_permisions.jpg)

**Restart NFS Server**

```
sudo systemctl restart nfs-server.service
```

![image](assets/nfs_server_44_restart_nfs_server.jpg)

6. Configure access to NFS for clients within the same subnet (example of Subnet CIDR - 172.31.32.0/20 ):

![image](assets/nfs_server_45_subnet_cdir.jpg)

```
sudo vim /etc/exports
```
**Add the following lines**:

```
/mnt/apps 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/opt 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
```

![image](assets/nfs_server_46_edit_nfs_exports.jpg)

**save and exit from the editor by** `Esc + :wq!`

**Export the NFS Shares**:

```
sudo exportfs -arv
```

![image](assets/nfs_server_48_export.jpg)


7. Check which port is used by NFS and open necessary ports in Security Groups (add new Inbound Rule)

**Check NFS Ports**:

```
rpcinfo -p | grep nfs
```

![image](assets/nfs_server_49_check%20ports.jpg)

> **Important note**: In order for NFS server to be accessible from our client,we open following ports:
- `TCP 111`
- `UDP 111`
- `UDP 2049`
- `UDP 2049`

![image](assets/port_range.jpg)

## Step 2 - Configure the database server

Log to aws account console and create EC2 instance of t2.micro type with Ubuntu Server launch in the default region us-east-1. name instance MySQL server

![image](assets/db_server_1_instance_name.jpg)

 Follow the same step and finally the instance created like this 

![image](assets/db_server_6_instance_details.jpg)

1. Install MySQL Server

First, we need to connect to our  Mysql server server

![image](assets/db_server_7_connect.jpg)

1.1. Update the package index:
```
sudo yum update
```
![image](assets/db_server_8_update.jpg)

1.2. Install MySQL server:
```
sudo yum install mysql-server
```
![image](assets/db_server_9_install_mysql_srver.jpg)

1.3 Check status :
```
sudo systemctl status mysqld
```
![image](assets/db_server_13_check_mysql_running.jpg)

1.4. Enable MySQL to start on boot:

```
sudo systemctl enable mysqld
```

![image](assets/db_server_12_enable_mysql.jpg)

1.5. Secure the MySQL installation (set root password, remove test databases, etc.):
```
sudo mysql_secure_installation
```
Follow the prompts to complete the configuration.

![image](assets/db_server_14_mysql_secure_instalation.jpg)


2. Create a database and name it tooling

 2.1. Log in to the MySQL server as the root user:

```
sudo mysql -u root -p
```
![images](assets/db_server_15_login.jpg)

2.2. Create the database:

```
CREATE DATABASE tooling;
```

![image](assets/db_server_16_tooling_database.jpg)

```
show databases;
```

![image](assets/db_server_17_show_databases.jpg)

3. Create a database user and name it `webaccess`

```
CREATE USER 'webaccess'@'%' IDENTIFIED BY 'PassWord.1';
```
![image](assets/db_server_18_create_user_webaccess.jpg)

4. Grant permission to webaccess user on tooling database to do anything only from the webservers subnet `cidr`
> To grant permissions to a MySQL user based on a specific subnet (CIDR), you can use a series of wildcard characters to match the IP address range. MySQL does not support direct CIDR notation, so you need to translate the CIDR range into a pattern that MySQL understands.

```
GRANT ALL PRIVILEGES ON tooling. * TO 'webaccess'@'%' WITH GRANT OPTION;
```

![images](assets/db_server_19_grantr_previlege.jpg)

**Apply the changes**:

```
FLUSH PRIVILEGES;
```

![image](assets/db_server_20_flush_privileges.jpg)

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

![image](assets/web_server_instance_name.jpg)

![image](assets/web_server_2_select_os.jpg)

![image](assets/web_server_3_key_pair.jpg)


**Follow same steps above and  our instance detail look like this**

![image](assets/db_server_6_instance_details.jpg)


2. Install NFS client
```
sudo yum update -y
```
![image](assets/web_server_6_update.jpg)


```
sudo yum install nfs-utils nfs4-acl-tools -y
```
![image](assets/web_server_8_install_client.JPG)

3. Mount `/var/www/` and target the NFS server's export for apps

```
sudo mkdir /var/www
```

![images](assets/web_server_9_create_www_directory.JPG)

```
sudo mount -t nfs -o rw,nosuid 172.31.36.105:/mnt/apps /var/www
```

![image](assets/web_Server_10_mount.jpg)

4. Verify that NFS was mounted successfully

```
 df -h
 ```

![image](assets/web_server_11_initials.jpg)
  
Make sure that the changes will persist on Web Server after reboot:

```
sudo vi /etc/fstab
```

add following line

```
172.31.36.105:/mnt/apps /var/www nfs defaults 0 0
```
![image](assets/web_server_12.jpg)

5. Install Remi's repository, Apache and PHP

```
sudo yum install httpd -y
```

![image](assets/web_server_13_install_httpd.jpg)

```
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```
![image](assets/web_server_14_install_all.jpg)

**To confirm that EPEL has been added**
```
 rpm -qi epel-release
```
![image](assets/web_server_15_install.jpg)

```
sudo dnf -y install http://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```
![image](assets/web_server_16_remi-release-9.jpg)

> If you encounter the "Killed" message while running , it likely means the process was terminated due to running out of memory. Here are steps to mitigate this:
Updating System with Limited Memory

Create a swap file to increase virtual memory:


```
sudo fallocate -l 1G /swapfile
sudo dd if=/dev/zero of=/swapfile bs=1M count=512
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```
![images](assets/swap%20file.jpg)

**Make the swap file permanent:**

```
echo '/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab
```

![image](assets/web_server_17_.jpg)

**Update and Clean the System and re run**

```
sudo dnf upgrade --refresh -y
sudo dnf upgrade -y dnf
sudo dnf update -y
sudo dnf clean all
sudo reboot
```

![image](assets/web_server_18_.jpg)

**Tehn Re run again**

```
sudo dnf -y install http://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```

![image](assets/web_server_19_updates.jpg)

Before installing PHP, we need to check the available PHP streams in the repository.

```
 sudo dnf module list php -y
```

![image](assets/web_server_20_.jpg)

```
sudo dnf module reset php
```

![image](assets/web_server_21_.jpg)

```
sudo dnf module enable php:remi-8.2 -y
```

![image](assets/web_server_22_.jpg)

```
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
```

![image](assets/web_server_23_.jpg)

```
php -v
```

![image](assets/web_server_24_.jpg)

```
sudo systemctl start php-fpm
```

![image](assets/web_server_25_.jpg)


```
sudo systemctl enable php-fpm
```

![image](assets/web_server_26_.jpg)


```
sudo setsebool -P httpd_execmem 1
```
![image](assets/web_server_12_install_boolean.jpg)


> Repeat steps 1-5 for another 2 Web Servers and finally 

![image](assets/web_servers_2.jpg)





6. Verify NFS Mount and Apache Setup 

Verify that Apache files and directories are available on the Web Server in `/var/www `and also on the NFS server in `/mnt/apps`. If you see the same files - it means NFS is mounted correctly.

```
cd /var/www
```
![image](assets/final_1.jpg)

```
ls -l
```
![image](assets/final_2.jpg)

 You can try to create a new file
```
sudo touch test.txt
```
![image](assets/final_3.jpg)



**We can see the text.txt file created inside our nfs server `/mnt/apps` directory. So they are communicating perfectly.**

![image](assets/final_4.jpg)

from one server and check if the same file is accessible from other Web Servers.

![image](assets/final_5.jpg)


7. Locate the log folder for Apache on the Web Server and mount it to NFS server's export for logs. Repeat step №4 to make sure the mount point will persist after reboot.
```
sudo mkdir -p /var/log/httpd
```
![image](assets/web_serv_final_1.jpg)

```
sudo mount -t nfs -o rw,nosuid 172.31.36.105:/mnt/logs /var/log/httpd
```
![image](assets/web_serv_final_2.jpg)

Edit the /etc/fstab file so that it persists even after reboot
```
sudo vi /etc/fstab
```
add the following lines
```
172.31.36.105:/mnt/logs /var/log/httpd nfs defaults 0 0
```
![image](assets/web_serv_final_3.jpg)

8. Fork the tooling source code from Darey.io Github Account to your Github account.
**Download git**
```
sudo yum install git
```
![image](assets/web_serv_final_5_instal_git.jpg)

**Clone the repository you forked the project into**
```
git clone https://github.com/gashawgedef/tooling.git
```
![image](assets/web_serv_final_6.jpg)

9. Deploy the tooling website's code to the Webserver. Ensure that the html folder from the repository is deployed to /var/www/html
```
sudo cp -R html/. /var/www/htm/
```
![image](assets/web_serv_final_7.jpg)

> Note 1: Do not forget to open TCP port 80 on the Web Server.

> Note 2: If you encounter 403 Error - check permissions to your /var/www/html folder and also disable SELinux sudo setenforce 0 To make this change permanent - open following config file

**Disable SELinux**
```
sudo setenforce 0
```
![image](assets/web_serv_final_8.jpg)

```
sudo vi /etc/sysconfig/selinux
```
and set SELINUX=disabled

![image](assets/web_serv_final_9.jpg)

 **Then restrt httpd**.
```
sudo systemctl restart httpd
```
![image](assets/web_serv_final_10.jpg)
```
sudo systemctl status httpd
```
![image](assets/web_serv_final_11.jpg)


10. Update the website's configuration to connect to the database (in /var/www/html/functions.php file). Apply `tooling-db.sql` script to your database using this command
 
![image](assets/web_serv_final_12.jpg)

 ```
  mysql -h 172.31.37.78 -u webaccess -p -D tooling < tooling-db.sql

```
![image](assets/web_serv_final_13.jpg)


11. Create in MySQL a new admin user with `username: myuser` and `password: password`:

```
mysql -h 172.31.37.78 -u webaccess -p
```
![image](assets/web_serv_final_14.jpg)

```
INSERT INTO `users` (`id`, `username`, `password`, `email`, `user_type`, `status`) 
VALUES (2, 'webaccess_user', '5f4dcc3b5aa765d61d8327deb882cf99', 'user@mail.com', 'admin', '1');

```
![image](assets/web_serv_final_15.jpg)

12. Open the website in your browser http://`Web-Server-Public-IP-Address`or `Public-DNS-Name`/index.php and make sure you can login into the website with  your user and password.

![image](assets/web_serv_final_16.jpg)

![image](assets/web_serv_final_17.jpg)


### End of the Project
We successfully implemented and deployed a DevOps tooling website in AWS, designed to provide seamless access to essential DevOps tools within the corporate infrastructure. The project involved setting up three web servers, all connected to a central database server. Additionally, a Network File System (NFS) was configured to enable the web servers to share common files, ensuring synchronized access to data across the infrastructure. This scalable solution enhances the efficiency of DevOps operations by integrating a reliable, high-availability system for managing DevOps tools.

