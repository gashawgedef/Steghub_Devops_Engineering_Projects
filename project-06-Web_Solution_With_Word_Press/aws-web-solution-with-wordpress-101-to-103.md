# Web Solution With WordPress

In this project we will be tasked to prepare storage infrastructure on `two` **Linux servers** and implement a basic web solution using `WordPress`. **WordPress** is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).

## Project 6 consists of two parts:

1. Configure storage subsystem for Web and Database servers based on Linux OS. The focus of this part is to give us practical experience of working with disks, partitions and volumes in Linux.
2. Install WordPress and connect it to a remote MySQL database server. This part of the project will solidify our skills of deploying Web and DB tiers of Web solution.

### Three-tier Architecture
Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.

Three-tier Architecture is a client-server software architecture pattern that comprise of 3 separate layers.


1. **Presentation Layer (PL)**: This is the user interface such as the client server or browser on your laptop.
2. **Business Layer (BL)**: This is the backend program that implements business logic. Application or Webserver
3. **Data Access or Management Layer (DAL)**: This is the layer for computer data storage and data access. Database Server or File System Server such as File Transfer Protocol `FTP` server, orNetwork File System `NFS` Server

![Images](assets/3_tier_architecture1.jpg)

### Our 3-Tier Setup
1. A Laptop or PC to serve as a client
2. An EC2 Linux Server as a web server (This is where you will install WordPress)
3. An EC2 Linux server as a database (DB) server

> Use RedHat OS for this projects we will use very popular distribution called 'RedHat' (it also has a fully compatible derivative - CentOS)


**Let us get started by creating `Web Server` and `Database Server` using redhat Os**

1. Launch Two EC2 instances that will serve as **`Web Server`** and **`DB Server`**.


![image](assets/web_server_1_name.JPG)

2. Application and OS Images select RedHat free tire eligable version

![image](assets/web_server_2_os.JPG)

3. Create new key pair or select existing key

![image](assets/web_server_3_key_pair.JPG)

4. Network setting create new security group or use existing security group

![image](assets/web_server_4_security_group.JPG)

5. Configure Storage and launch the instance

![image](assets/web_server_5_configure_storage.JPG)

   
6. View Instance

   ![image](assets/web_server_7_view_instance.JPG)

7. Instance Details for `Web Server`

![image](assets/web_server_8_view_details.JPG)

   
8. Configure security group with the following inbound rules:

- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default.
- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.

![image](assets/web_server_10_configure%20ports.JPG)

### For DB Server follow same steps and our final instance detail looks like this

- `DB Server` instance after launched looks like

![image](assets/db_server_6_view_instance.JPG)

- `DB Server` instance Details Looks like:

![image](assets/db_server_7_view_details.JPG)


### Now Two Servers `running` and let's configure our 3-Tire  server architecture

![image](assets/up_servers.JPG)

> **Note**: for Ubuntu server, when connecting to it via SSH/Putty or any other tool, we used ubuntu user, but for RedHat you will need to use ec2-user user. Connection string will look like ec2-user@<Public-IP>

**Let us see how to connect webserver**

![image](assets/web_server_connect_1.JPG)


![image](assets/web_server_connect_2.JPG)

![image](assets/web_server_connect_3.JPG)

# Step 1 — Prepare a Web Server

Launch an EC2 instance that will serve as `Web Server`. Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

1. Add EBS Volume to an EC2 instance

![image](assets/web_server_create_vol_1_0.JPG)

![image](assets/web_server_create_vol_1_0.JPG)

![image](assets/web_server_create_vol_2.JPG)


**Created volumes in same availablity zone looks like this**

![image](assets/web_server_volumes_list.JPG)


2. Attach all three volumes one by one to our Web Server EC2 instance

![image](assets/atach_volume1.JPG)

![image](assets/atach_volume_2.JPG)

![image](assets/atach_volume_3.JPG)

**After three voumes attached to the webserver instance**

![image](assets/volume_list_in_use.JPG)

3. Open up the Linux terminal to begin configuration

![image](assets/web_server_11_lsblk.JPG)


4. Use `lsblk` command to inspect what block devices are attached to the server.
```
lsblk
```
![image](assets/web_server_12_lsblk_results.JPG)

5. Use df -h command to see all mounts and free space 
```
df -h
```
![image](assets/web_server_13_free_mount_space.JPG)


6. Use `gdisk` utility to create a single partition on each of the 3 disks

```
sudo gdisk /dev/xvdb
```
**List Existing Partitions: To see the current partitions, use the p command:**

![image](assets/web_server_14_create+partition_1.JPG)

**Create a New Partition: To add a new partition, enter n: then Press Enter to accept default value**

![image](assets/web_server_15_create_partition_2.JPG)

**Write Changes: Once you've created the desired partitions, write the changes to the disk with w:**

![image](assets/web_server_16_create+partition_3.JPG)


**Yes to proceed and complete** 
![image](assets/web_server_17_create+partion_4.JPG)

![image](assets/web_server_19_create.JPG)

![image](assets/web_server_20_final.JPG)

**we follow the same steps for remaining two partitions**

7. Use lsblk utility to view the newly configured partition on each of the 3 disks.
```
lsblk
```
![image](assets/web_server_21_after_create_three_partitions.JPG)

8. Install `lvm2` package 
```
sudo yum install lvm2
```
![image](assets/web_server_22_install_lvm2.JPG)


9.  Check for available partitions.
```
sudo lvmdiskscan 
```
![image](assets/web_server_23_check_lvmdiskscan.JPG)

> Note:  In Ubuntu we used apt command to install packages, in **RedHat/CentOS** a different package manager is used, so we shall use `yum` command instead.

10. Use `pvcreate` utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
```
sudo pvcreate /dev/xvdb1
```
![image](assets/web_server_24_phical_volume_xvdb1.JPG)

```
sudo pvcreate /dev/xvdc1
```
![image](assets/web_server_25_phisical_volume_xvdc1.JPG)

```
sudo pvcreate /dev/xvdd1
```
![image](assets/web_server_26_phisical_volume_xvdd1.JPG)

11. Verify that your Physical volume has been created successfully 

```
sudo pvs
```
![image](assets/web_server_27_verify_phisical_volume_created.JPG)

12. Use `vgcreate` utility to add all 3 PVs to a volume group (VG) Name the VG `webdata-vg`
```
sudo vgcreate webdata-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
![image](assets/web_server_28_create_volume_group.JPG)

13. Verify that your VG has been created successfully
```
 sudo vgs
``` 
![image](assets/web_server_299_verify_group.JPG)

14.  Use `lvcreate` utility to create 2 logical volumes. `apps-lv` (**Use half of the PV size**), and `logs-lv` Use the remaining space of the PV size.

>  NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.
```
sudo lvcreate -n apps-lv -L 14G webdata-vg
```
![image](assets/web_server_30_logical_volume_apps_created.JPG)


```
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
![image](assets/web_server_31_logical_volume_logs_lv_created.JPG)

15. Verify that our Logical Volume has been created successfully
```
sudo lvs
```
![image](assets/web_server_32_verify_logical_volumes_created.JPG)

16.  Verify the entire setup
#view complete setup - VG , PV, and LV
```
sudo vgdisplay -v
```
![image](assets/web_server_33_verify_entire_access.JPG)

```
sudo lsblk
```
![image](assets/web_server_34_show_all_partitions.JPG)

**Use mkfs.ext4 to format the logical volumes with ext4 filesystem**
```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
```
![image](assets/web_server_35_format_ext4_1.JPG)


```
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
![image](assets/web_server_36_format_ext4_2.JPG)

17. Create `/var/www/html` directory to store website files
```
sudo mkdir -p /var/www/html
```
![image](assets/web_server_37_create_html.JPG)

18. Create `/home/recovery/logs` to store backup of log data 

```
sudo mkdir -p /home/recovery/logs
```
![image](assets/web_server_38_create_logs.JPG)


19.  Mount `/var/www/html` on apps-lv logical volume

```
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```
![image](assets/web_server_39_mount.JPG)

**Verify the Mount:**
```
df -h | grep /var/www/html
```
![image](assets/web_server_40_verify_mount.JPG)

20. Use rsync utility to backup all the files in the log directory `/var/log` into `/home/recovery/logs`
 
 > This is required before mounting the file system

```
sudo rsync -av /var/log/ /home/recovery/logs/
```
![image](assets/web_server_41_rsync.JPG)

21. Mount `/var/log` on `logs-lv` logical volume
    
> Note that all the existing data on `/var/log` will be deleted. That is why step 17 above is very important

```
sudo mount /dev/webdata-vg/logs-lv /var/log
```
![image](assets/web_server_42_mount.JPG)


22. Restore log files back into `/var/log` directory
```   
sudo rsync -av /home/recovery/logs/ /var/log
```
![image](assets/web_server_43_restore_log_files.JPG)


23. Update `/etc/fstab` file so that the mount configuration will persist after restart of the server.  The UUID of the device will be used to update the `/etc/`fstab file;

**Find the UUID of the Device**
```
sudo blkid
```
![image](assets/web_server_44_find_UIDD_device.JPG)

**Edit the `/etc/fstab` File**
```
sudo vim /etc/fstab
```
![image](assets/web_server_45_open_fstap.JPG)

**Replace UUID with the actual UUID from the blkid**
```
# MOUNTS FOR WORDPRESS WEBSERVER
UUID=481d6b40-7d14-4897-bbce-5cb1db497ac1 /var/www/html    ext4 defaults 0 0
UUID=99c0c884-1094-4b8a-a122-08b0b000d01d  /var/log         ext4 defaults 0 0
```
![image](assets/web_server_46_edit_fstab.JPG)

24. Test the configuration 
```
sudo mount -a
```
![image](assets/web_server_47_test_configuration.JPG)

**Reload the daemon**
```
sudo systemctl daemon-reload
```
![image](assets/web_server_48_test_configuration_1.JPG)

25. Verify our setup
```
df -h
```
![image](assets/web_server_48_verify_set_up.JPG)

# Step 2 - Prepare the Database Server
1. Launch A second RedHat EC2 Instance that will have a role **`DB Server`**
Repeat the same steps as for the Web Server, but instead of _**`apps-lv`**_ create _**`db-lv`**_ and mount it to _**`/db`**_  directory instead of **`/var/www/html/`**

### Afterfollowing same step Like  `Web Server` we created an instance ` DB Server` and looks like  this 

![image](assets/db_server_6_view_instance.JPG)

**Now we are going to Add EBS Volume to an DB Server EC2 instance we repeat the above steps**
1. Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

![image](assets/web_server_49_db_server_volumes_created.jpg)

**Attach all volumes one by one to our DB Server EC2 instance**
![image](assets/web_server_50_db_after_atached.jpg)

2. Open up the Linux terminal to begin configuration

![image](assets/db_server_8_give_permision.jpg)

Connect with `DB Server` Instance

![image](assets/db_server_9_connect.jpg)

3. Use lsblk command to inspect what block devices are attached to the server.

```
lsblk
```
![image](assets/db_server_10_lsblk.jpg)


4. Use df -h command to see all mounts and free space on your server
   ```
   df -h
   ```
![image](assets/db_server_11_see_all_mounts_free_space.jpg)

 **Use gdisk utility to create a single partition on each of the 3 disks**
``` 
sudo gdisk /dev/xvdb
```
**List Existing Partitions: To see the current partitions, use the p command:**
![image](assets/db_server_12_create_partition_1.jpg)

**Create a New Partition: To add a new partition, enter `n`: then Press Enter to accept default value**
![image](assets/db_server_13_create_partition_2.jpg)


**Write Changes: Once you've created the desired partitions, write the changes to the disk with `w`**:
![image](assets/db_server_14_create_partition_3.jpg)


**Yes to proceed and complete**
![image](assets/db_server_15_procced_yes.jpg)

Finally the partition is created Like:

![image](assets/db_server_16_partition_full_step.jpg)

**we follow the same steps for remaining**

5. Use **lsblk** utility to view the newly configured partition on each.
```
lsblk
```
![image](assets/db_server_17_after_partitions.jpg)


6. Install **lvm2** package
```
sudo yum install lvm2
```
![image](assets/db_server_18_install_lvm2_1.jpg)

![image](assets/db_server_18_install_lvm2_1.jpg)

**Check for available partitions**
```
sudo lvmdiskscan
```
![image](assets/db_server_check_for_available_partitions.jpg)

7. Use pvcreate utility to mark each of disks as physical volumes (PVs) to be used by LVM
```
sudo pvcreate /dev/xvdb1
sudo pvcreate /dev/xvdc1
sudo pvcreate /dev/xvdd1
```
![image](assets/db_server_20_pvcreate.jpg)

8. Verify that our Physical volume has been created successfully
```
sudo pvs
```
![image](assets/db_server_21_check_p_v_created.jpg)

9. Use **vgcreate** utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg
```
sudo vgcreate webdata-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
![image](assets/db_server_22_vg_create.jpg)

10. Verify that our VG has been created successfully
```
sudo vgs
```
![image](assets/db_server_23_verify_volume_group.jpg)

11. Use **lvcreate** utility to create 2 logical volumes. db-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size.
> NOTE: db-lv will be used to store data for the database while, logs-lv will be used to store data for logs.
```
sudo lvcreate -n dbs-lv -L 14G webdata-vg
```
![image](assets/db_server_24_dbs_lv.jpg)

```
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
![image](assets/db_server_25_logs_lv.jpg)

12. Verify that our Logical Volume has been created successfully
```
sudo lvs
```
![image](assets/db_server_26_verify_lv.jpg)

13. Verify the entire setup view complete setup - VG, PV, and LV
```
sudo vgdisplay -v
```
![image](assets/db_server_27_verify_complete_setup.jpg)

```
sudo lsblk 
```
![image](assets/db_server_28_all_available_partitions.jpg)

**Use mkfs.ext4 to format the logical volumes with ext4 filesystem**
```
sudo mkfs -t ext4 /dev/webdata-vg/dbs-lv
```
![image](assets/db_server_29_dbs_lv.JPG)

```
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
![image](assets/db_server_30_logs_lv.JPG)


14. Create `/db` directory to store database files
```
sudo mkdir -p /db
```
![image](assets/db_server_31_db_directory.JPG)

16. Create `/home/recovery/logs` to store backup of log data
```
 sudo mkdir -p /home/recovery/logs
```
![image](assets/db_server_32_logs_directory.JPG)

17. Mount `/db `on `dbs-lv` logical volume
```
sudo mount /dev/webdata-vg/dbs-lv /db/
```
![image](assets/db_server_33_mount.JPG)

**Verify the Mount**:
```
df -h | grep /db
```
![image](assets/db_server_34_verify_mount.JPG)

18. Use **rsync** utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)
```
sudo rsync -av /var/log/ /home/recovery/logs/
```
![image](assets/db_server_35_rsync.JPG)

19. Mount `/var/log` on `logs-lv` logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important)
```
sudo mount /dev/webdata-vg/logs-lv /var/log
```
![image](assets/db_server_36_mount_logical_volume.JPG)

20. Restore log files back into /var/log directory
```
sudo rsync -av /home/recovery/logs/log/. /var/log
```
![image](assets/db_server_37_restore_log_files.JPG)

21. Update `/etc/fstab` file so that the mount configuration will persist after restart of the server. The UUID of the device will be used to update the `/etc/fstab` file;

**Find the UUID of the Device**
```
sudo blkid
```
![image](assets/db_server_38_blkid.JPG)

**Edit the /etc/fstab File**
```
sudo vi /etc/fstab
```
![image](assets/db_server_39_open_fstab.JPG)

**Update /etc/fstab in this format using our own UUID**
Replace UUID with the actual UUID from the blkid

```
UUID=d8247734-039f-48f4-a784-ff60fd905a88  /db             ext4 defaults 0 0
UUID=fe8c3fb4-b5cb-43d3-8821-625afdfe4cf7  /var/log         ext4 defaults 0 0
```
![image](assets/db_server_40_edit_fstab.JPG)

22. Test the configuration 
```
sudo mount -a
```
![image](assets/db_server_41_test_config_1.JPG)

**Reload the daemon**
```
sudo systemctl daemon-reload
```
![image](assets/db_server_42_reload_daemon.JPG)

23. Verify our setup by running
```
df -h
```
![image](assets/db_server_43_verify_setup.JPG)

# Step 3 - Install WordPress on your Web Server EC2
1. Update the repository

```
sudo yum -y update
```
![image](assets/step_3_update_webserver.JPG)

2. Install **`wget`**, **`Apache`** and it's dependencies
```
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
![image](assets/step_3_install_wget_apache.JPG)

3. Start Apache
**Enable httpd**
```
sudo systemctl enable httpd 
```
![image](assets/step_3_enable_httpd.JPG)

**start httpd**
```
sudo systemctl start httpd
```
![image](assets/step_3_start_httpd.JPG)

4. Install PHP and it's dependencies
 ```  
 sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```
![image](assets/step_3_instal_php_1.JPG)


```
sudo dnf -y install http://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```
![image](assets/step_3_instal_php_2.JPG)

**Before installing PHP, we need to check the available PHP streams in the repository.**
```
 sudo yum module list php 
```
![image](assets/step_3_install_php_3.JPG)

```
sudo yum module reset php 
```
![image](assets/step_3_install_php_4.JPG)

```
sudo dnf module enable php:remi-8.0
```
![image](assets/step_3_install_php_5.JPG)

```
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
```
![image](assets/step_3_install_php_6.JPG)


```
sudo systemctl start php-fpm
```
![image](assets/step_3_install_php_7.JPG)

```
sudo systemctl enable php-fpm
```
![image](assets/step_3_install_php_8.JPG)

```
sudo setsebool -P httpd_execmem 1
```
![image](assets/step_3_install_php_9.JPG)



5. Restart Apache and check it is running.
```
sudo systemctl restart httpd
```
![image](assets/step_3_restart_httpd.JPG)

```
sudo systemctl status httpd
```

![images](assets/step_3_check_ruuning.JPG)


6. Download wordpress and copy wordpress to `/var/www/html`

```
cd wordpress
```
![image](assets/step_3_install_word_press_1.JPG)

```
sudo wget http://wordpress.org/latest.tar.gz
```
![image](assets/step_3_wordpress_2.JPG)

```
 sudo tar -xzvf latest.tar.gz
```
![images](assets/step_3_wordpress_3.JPG)


```
sudo rm -rf latest.tar.gz 
```

![images](assets/step_3_wordpress_4.JPG)


```
cp wordpress/wp-config-sample.php wordpress/wp-config.php
```

![images](assets/step_3_wordpress_5.JPG)


```
sudo cp -R wordpress /var/www/html/
```
![images](assets/step_3_copy_1.JPG)

7. Configure SELinux Policies
```
sudo chown -R apache:apache /var/www/html/wordpress
```
![image](assets/step_3_conf1.JPG)

```
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
```
![image](assets/step_3_conf_2.JPG)

```
sudo setsebool -P httpd_can_network_connect=1
```
![image](assets/step_3_connect_1.JPG)


# Step 4 — Install MySQL on our DB Server EC2
**Update the system**

```
sudo yum update
```
![image](assets/step_4_update_1.JPG)

**Install Mysql Server**
```
sudo yum install mysql-server
```
![image](assets/step_4_install_mysql.JPG)

**Verify that the service is up and running**
```
sudo systemctl status mysqld
```
![image](assets/step_4_status_mysqld.JPG)

**if it is not running, restart the service.** 
```
sudo systemctl restart mysqld
```
![image](assets/step_4_restart_mysqld.JPG)

**Enable it to be running even after reboot**:
```
sudo systemctl enable mysqld
```
![image](assets/step_4_enable_mysqld.JPG)
**Check status now**
![image](assets/step_4_status_running.JPG)

# Step 5 - Configure DB to work with WordPress
**Log to mysql**
```
sudo mysql
```
![image](assets/step_5_login_mysql.JPG)

```
CREATE DATABASE wordpress;
```
![image](assets/step_5_create_database.JPG)

```
CREATE USER `gashaw`@`172.31.37.132` IDENTIFIED BY 'PassWord.1';
```
![image](assets/step_5_create_user.JPG)

```
GRANT ALL ON wordpress.* TO 'gashaw'@'172.31.37.132';
```
![image](assets/step_5_grant_previleges.JPG)

```
FLUSH PRIVILEGES;
```
![image](assets/step_5_flush_previleges.JPG)


```
SHOW DATABASES;
```
![image](assets/step_5_show_databases.JPG)

Enter `exit` and logout

```
exit
```

# Step 6 - Configure WordPress to connect to remote database

> Hint: Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, we shall allow access to the DB server ONLY from our Web Server's IP address so in the Inbound Rule configuration specify source as /32

![image](assets/step_6_configure_port.JPG)



1. Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client
```
sudo yum install mysql
```
![image](assets/step_6_mysql_install.JPG)


**sudo mysql -u admin -p -h <DB-Server-Private-IP-address>**
```
sudo mysql -u gashaw -p -h 172.31.34.75
```
![image](assets/step_6_login_mysql_from_client.JPG)


2. Verify if you can successfully connect
 ```
SHOW DATABASES;
```
![image](assets/step_6_show_databases.JPG)


**if sucessful the command list of existing databases.**

3. Change permissions and configuration so Apache could use WordPress:

4. Enable TCP port 80 in Inbound Rules configuration for our Web Server EC2 (enable from everywhere 0.0.0.0/0 or from our workstation's IP)

![image](assets/step_6_configure_port-one.JPG)

5. Try to access from your browser the link to your WordPress
```
http://<Web-Server-Public-IP-Address>/wordpress/
```
![image](assets/word_press_1.JPG)


![image](assets/wordpress_2.JPG)


![image](assets/word_press_login.JPG)

![image](assets/wordpress_dashboard.JPG)

### The End of Web Solution With WordPress project
In this project we prepared storage infrastructure on two Linux servers and implement a basic web solution using WordPress.
1. Configure storage subsystem for Web and Database servers based on Linux OS. The focus is working with disks, partitions and volumes in Linux
2. Install WordPress and connect it to a remote MySQL database server
3. Deploying Web and DB tiers of Web solution
