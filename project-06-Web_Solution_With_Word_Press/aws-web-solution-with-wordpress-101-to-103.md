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

# Step 1 — Prepare a Web Server

> Use RedHat OS for this projects we will use very popular distribution called 'RedHat' (it also has a fully compatible derivative - CentOS)


**Let us get started by creating web server and database server using redhat Os**

1. Launch an EC2 instance that will serve as "Web Server". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.


![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/99319c00-fd57-434d-b584-756bd396fbae)

2. Application and OS Images select RedHat free tire eligable version

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/47800606-58c2-4233-b7d4-19235461335e)

3. Create new key pair or select existing key

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/fb1410f5-33cf-4f1f-b6b8-bb43f1636cd6)

4. Network setting create new security group or use existing security group

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/64844d2b-56ac-4667-8f28-20e1823d95f5)

5. Configure Storage and launch the instance

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ae30ed2f-7b43-4679-94ed-6de5eba8a959)

   
6. View Instance

   ![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e05fc97c-ecd1-4a49-baa8-6e7e24056585)

7. Instance Details for web

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/371c3b99-edd5-4d80-a5be-25150d0d9ac5)

   
8. Configure security group with the following inbound rules:

- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default.
- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0b3dd6b9-527e-4db8-8fd6-509a4556abba)

### For DB Server follow same steps and our final instance detail looks like this
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e63bfe7c-9bcf-4e0c-bb9f-212194726d05)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/cff44690-f4f9-44df-97a4-6b6fdf622f7b)


### Now Two Servers UP are we can go to do our 3-Tire  server architecture

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d1bda6a5-a142-4ba6-bd7b-87ab9155971a)

> **Note**: for Ubuntu server, when connecting to it via SSH/Putty or any other tool, we used ubuntu user, but for RedHat you will need to use ec2-user user. Connection string will look like ec2-user@<Public-IP>

**Let us see how to connect webserver**

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/2730dd36-2176-4736-9587-2369c7c7a5b2)


![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/15bb6adc-d1e2-4e75-bb55-c2ba721a1b65)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/9ff07f1f-2fa8-47f9-8c83-b55ca964249c)

### Step 1 - Prepare a Web Server
Launch an EC2 instance that will serve as Web Server. Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

1. Add EBS Volume to an EC2 instance

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/47681df8-7b7c-410e-987a-fdb72e60844f)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d7516732-5aa5-4ba6-b57e-932c436a3e72)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/4192ee2d-fa09-465c-9aa8-4c63cb26a575)


**Created volumes in same availablity zone**

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c7739cff-53cd-4dfd-957d-c1d0e2204c34)


2. Attach all three volumes one by one to our Web Server EC2 instance

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/4752ae95-89ac-4829-a8ea-fa7827cb7acc)

**After three voumes attached to the webserver instance**

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3c191531-c56e-4540-a0ea-0a4f405f9ec5)

3. Open up the Linux terminal to begin configuration

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/88502c8d-ac9b-4d80-99ee-06a372432baf)


4. Use `lsblk` command to inspect what block devices are attached to the server.
```
lsblk
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5d815dde-fc74-4c2a-a7c1-0cca7e8d724b)

5. Use df -h command to see all mounts and free space 
```
df -h
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1108fc69-c1c1-403e-abce-d0db003e51e8)


6. Use `gdisk` utility to create a single partition on each of the 3 disks

```
sudo gdisk /dev/xvdb
```
**List Existing Partitions: To see the current partitions, use the p command:**

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/f77f78ad-6018-458b-94a2-c0770f89cc72)

**Create a New Partition: To add a new partition, enter n: then Press Enter to accept default value**

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/9bb8e08d-1386-4438-9afd-6850c2e32189)

**Write Changes: Once you've created the desired partitions, write the changes to the disk with w:**

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/77bbc089-b4f5-4f54-94e5-5a8362eb9e43)


**Yes to proceed and complete** 
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/9eb8faf4-4f71-497a-8480-81d26c513c08)

**we follow the same steps for remaining two**

7. Use lsblk utility to view the newly configured partition on each of the 3 disks.
```
lsblk
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3003793e-4891-45dc-9412-0ad016b6c7e6)

8. Install `lvm2` package 
```
sudo yum install lvm2
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d88545f4-321a-4f51-944b-5caa7e20982f)


9.  Check for available partitions.
```
sudo lvmdiskscan 
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/cf26fad7-5683-4c3c-9f26-e5b3693c7838)

> Note:  In Ubuntu we used apt command to install packages, in **RedHat/CentOS** a different package manager is used, so we shall use yum command instead.

10. Use `pvcreate` utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
```
sudo pvcreate /dev/xvdb1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e364a48b-2aff-47fe-a7a5-fd8c580c9e38)

```
sudo pvcreate /dev/xvdc1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1b8518c3-8a09-411a-9cd0-4e06e73931df)

```
sudo pvcreate /dev/xvdd1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d743a184-6c75-4229-ba3d-7890b2e3ad6a)

11. Verify that your Physical volume has been created successfully 

```
sudo pvs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/9a550cbc-7572-48cd-9722-6301c9f12644)

12. Use `vgcreate` utility to add all 3 PVs to a volume group (VG) Name the VG `webdata-vg`
```
sudo vgcreate webdata-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e14f1d1d-0674-4d3a-a29c-618c1296e3a9)

13. Verify that your VG has been created successfully
```
 sudo vgs
``` 
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ccfb186a-3c7a-4902-8cf7-5a7afaec46ad)

14.  Use `lvcreate` utility to create 2 logical volumes. `apps-lv` (**Use half of the PV size**), and `logs-lv` Use the remaining space of the PV size.

>  NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.
```
sudo lvcreate -n apps-lv -L 14G webdata-vg
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a87d734f-fb9d-4542-8cee-8f21ccc0b02c)


```
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/707742d5-a8fa-4107-ac58-789be223aa82)

15. Verify that our Logical Volume has been created successfully
```
sudo lvs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/622c094c-74ae-40fe-9ee8-c9fae70a78b0)

16.  Verify the entire setup
#view complete setup - VG , PV, and LV
```
sudo vgdisplay -v
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6f4aab54-43f0-4cde-bc02-da88c8915ac0)

```
sudo lsblk
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a7f48bd6-f109-4df0-a1e4-a09cb84b513f)

**Use mkfs.ext4 to format the logical volumes with ext4 filesystem**
```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/14038c4e-9f99-4eff-8d78-ef22431c3d3c)


```
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/51302ae9-5a4f-4e85-8b29-938594806d46)

17. Create /var/www/html directory to store website files
```
sudo mkdir -p /var/www/html
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/91168896-ff99-4376-b3f7-75577fd2b182)

18. Create /home/recovery/logs to store backup of log data 

```
sudo mkdir -p /home/recovery/logs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8242fda2-9fbb-40c1-bac1-c445df3c086f)


19.  Mount /var/www/html on apps-lv logical volume

```
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5d0e29f1-387e-4194-a11a-d9a9bf517748)

**Verify the Mount:**
```
df -h | grep /var/www/html
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8415e922-a46c-46d3-8dad-02a869f63a1f)

20. Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs
 
 > This is required before mounting the file system

```
sudo rsync -av /var/log/. /home/recovery/logs/
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/cf6cc3ab-d945-434b-b4ea-edb009f50ef1)

21. Mount /var/log on logs-lv logical volume
    
> Note that all the existing data on /var/log will be deleted. That is why step 17 above is very important

```
sudo mount /dev/webdata-vg/logs-lv /var/log
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/65e718ac-f045-4fd3-8b4f-7218bf1d1aee)


22. Restore log files back into /var/log directory
```   
sudo rsync -av /home/recovery/logs/. /var/log
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ff8565d9-940a-4f74-a8e5-db2374d520c2)


23. Update /etc/fstab file so that the mount configuration will persist after restart of the server.  The UUID of the device will be used to update the /etc/fstab file;

**Find the UUID of the Device**
```
sudo blkid
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5a68ad7d-16e2-4532-9929-d992f5368701)

**Edit the /etc/fstab File**
```
sudo vim /etc/fstab
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/595b664d-3d08-4c21-b409-b060c1f68f4e)

**Replace UUID with the actual UUID from the blkid**
```
# MOUNTS FOR WORDPRESS WEBSERVER
UUID=f5c3bc97-925c-4692-b634-b217f65fb96e  /var/www/html    ext4 defaults 0 0
UUID=fc107995-52e8-44ae-b99f-b23f97aa54c8  /var/log         ext4 defaults 0 0
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/24041c0f-3e37-4b11-aa3b-89bf3ef748ca)

24. Test the configuration 
```
sudo mount -a
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/eec0837c-ec36-4a99-a7e9-6200ff046693)

**Reload the daemon**
```
sudo systemctl daemon-reload
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/4c9fb5bb-b7e3-42ca-bcbb-a59890f66d5f)

25. Verify our setup
```
df -h
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c9fcccfb-5fe9-45fb-8e2c-54fe21ca2965)

# Step 2 - Prepare the Database Server
1. Launch A second RedHat EC2 Instance that will have a role **`DB Server`**
Repeat the same steps as for the Web Server, but instead of _**apps-lv**_ create _**db-lv**_ and mount it to _**/db**_  directory instead of **/var/www/html/**

### Afterfollowing same step to create instance our DB Server look like this 

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e63bfe7c-9bcf-4e0c-bb9f-212194726d05)

**Now we are going to Add EBS Volume to an DB Server EC2 instance we repate the above steps**
1. Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5840d6f3-ac43-4213-8b02-4b861d779168)

**Attach all volumes one by one to our DB Server EC2 instance**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b751aad6-88c7-4be3-b7b4-0abc4e85e5c3)

2. Open up the Linux terminal to begin configuration

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/68ad3966-c3ec-45cd-a5d3-594be9e2bc9e)

3. Use lsblk command to inspect what block devices are attached to the server.

```
lsblk
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/f4d9f95f-9b70-4c61-abc6-2942d35cd27e)


4. Use df -h command to see all mounts and free space on your server
   ```
   df -h
   ```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/087e3729-3e92-477e-9a7b-632b0e9ef9cb)

 **Use gdisk utility to create a single partition on each of the 3 disks**
``` 
sudo gdisk /dev/xvdb
```
**List Existing Partitions: To see the current partitions, use the p command:**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/486ec823-230f-47d1-9add-37cb94f6f525)

**Create a New Partition: To add a new partition, enter n: then Press Enter to accept default value**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/70985507-987d-4dc2-a07b-6e670517444c)


**Write Changes: Once you've created the desired partitions, write the changes to the disk with w**:
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/fe18f1c1-ff33-40ce-9943-8895e8065aa7)


**Yes to proceed and complete**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8e01e914-1a10-4399-8ba5-871708c683af)

**we follow the same steps for remaining**

5. Use **lsblk** utility to view the newly configured partition on each.
```
lsblk
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a7992e1e-c54c-4fc6-a818-b40cb51258c4)


6. Install **lvm2** package
```
sudo yum install lvm2
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a4e922a2-730c-4a8b-a2b1-584c63191c15)

**Check for available partitions**
```
sudo lvmdiskscan
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/158cb1e7-7280-4bb4-ba19-bb23ee6e84b2)

7. Use pvcreate utility to mark each of disks as physical volumes (PVs) to be used by LVM
```
sudo pvcreate /dev/xvdb1
sudo pvcreate /dev/xvdc1
sudo pvcreate /dev/xvdd1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/01490ff3-6343-4521-b9de-e4f477665cc3)

8. Verify that our Physical volume has been created successfully
```
sudo pvs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a964f5a6-258f-4a9c-b725-840858b8820b)

9. Use **vgcreate** utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg
```
sudo vgcreate webdata-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/fbe5e6f5-91ab-485b-8129-2416931966a1)

10. Verify that our VG has been created successfully
```
sudo vgs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/41559f26-47c2-4be0-a2b8-e5f2937ccbfc)

11. Use **lvcreate** utility to create 2 logical volumes. db-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size.
> NOTE: db-lv will be used to store data for the database while, logs-lv will be used to store data for logs.
```
sudo lvcreate -n dbs-lv -L 14G webdata-vg
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7f1a14c3-7588-435d-902f-2e0a246483c5)

```
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e619c79a-36e8-441e-933f-679185b031d6)

12. Verify that our Logical Volume has been created successfully
```
sudo lvs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1ac12b5f-f75c-4451-ad6a-14541afd0993)

13. Verify the entire setup view complete setup - VG, PV, and LV
```
sudo vgdisplay -v
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8918761a-512f-4f45-8b43-b7746e89908e)

```
sudo lsblk 
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0c1fcc6a-41cb-4b6f-b364-2c3304b1f246)

**Use mkfs.ext4 to format the logical volumes with ext4 filesystem**
```
sudo mkfs -t ext4 /dev/webdata-vg/dbs-lv
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d91c4c55-b31f-4236-8b2e-49e32e3a8207)

```
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c538d568-3d5d-49f2-a48c-c899da5162a8)


14. Create /db directory to store database files
```
sudo mkdir -p /db
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/9d2af98a-fdf0-488d-9e50-fce451ed3018)

16. Create /home/recovery/logs to store backup of log data
```
 sudo mkdir -p /home/recovery/logs
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5ca67075-4a7e-4931-a235-8cb83ec28dc4)

17. Mount /db on dbs-lv logical volume
```
sudo mount /dev/webdata-vg/dbs-lv /db/
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6b3dd65a-1385-42cb-8f62-cdd2c529139b)

**Verify the Mount**:
```
df -h | grep /db
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6b6d4454-e347-4775-9b0b-292597bb16c1)

18. Use **rsync** utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)
```
sudo rsync -av /var/log/. /home/recovery/logs/
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/98c42855-0295-4117-96db-144e357682d9)

19. Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important)
```
sudo mount /dev/webdata-vg/logs-lv /var/log
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/cec4db6e-66eb-4ea3-823f-93b90650a228)

20. Restore log files back into /var/log directory
```
sudo rsync -av /home/recovery/logs/log/. /var/log
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1529e9f7-b2e2-422c-b1f0-50b2e3ed634b)

21. Update /etc/fstab file so that the mount configuration will persist after restart of the server. The UUID of the device will be used to update the /etc/fstab file;

**Find the UUID of the Device**
```
sudo blkid
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6db3bb48-8358-4bd0-958b-355d265e68d2)

**Edit the /etc/fstab File**
```
sudo vi /etc/fstab
```

**Update /etc/fstab in this format using our own UUID**
Replace UUID with the actual UUID from the blkid
```
UUID=7a63eca8-a7e7-4400-9a0e-1d3e5a03f913  /db             ext4 defaults 0 0
UUID=a042a1ca-fd09-498e-bf99-7977bd1aff23  /var/log         ext4 defaults 0 0
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b1d5378e-821a-493a-bfc5-9c58973fc3b6)

22. Test the configuration 
```
sudo mount -a
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/960179bb-5bea-4cca-af87-0f8fbe9eb9b1)

**Reload the daemon**
```
sudo systemctl daemon-reload
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8fc9f348-5edc-4e27-90e6-d8d857fc6aaa)

23. Verify our setup by running
```
df -h
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/f1494bee-e61b-4b92-b712-257689ee35db)

# Step 3 - Install WordPress on your Web Server EC2
1. Update the repository

```
sudo yum -y update
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/89ea9d94-f1b3-4366-ab19-db6de8ff14bd)

2. Install **wget**, **Apache** and it's dependencies
```
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/48da8b05-c3ff-4649-a3a6-6a6c2ca58f54)

3. Start Apache
**Enable httpd**
```
sudo systemctl enable httpd 
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3fc7ac23-64d5-4c35-9533-ef470994b28f)

**start httpd**
```
sudo systemctl start httpd
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b6b4a789-bd77-413d-95a9-1531f51c87e3)

4. Install PHP and it's dependencies
 ```  
 sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ec113dca-4fe7-4758-ae90-721f837d172d)

**To confirm that EPEL has been added**

```
 rpm -qi epel-release
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/df707e19-63cc-49cd-865a-e1992efeb464)

```
sudo dnf -y install http://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/bf9250cb-578e-4776-bb5a-af6ca7579167)

**Before installing PHP, we need to check the available PHP streams in the repository.**
```
 sudo dnf module list php -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5f219412-4b1e-4872-998c-c00152c16a8d)

```
sudo dnf module reset php -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7c7a8a71-15c1-4f99-bbcb-bd5b313d2b9e)

```
sudo dnf module enable php:remi-8.2 -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/452daf6f-262a-4d98-b9dc-86003ca0b835)

```
sudo dnf install php -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/fd8e4852-4494-47f0-96b8-a2623eaa8c83)

```
php -v
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/f6cb5b4e-c3de-41b3-8277-d5c4fe6e7320)

```
sudo systemctl start php-fpm
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3870cf6c-5896-4d35-b500-3291a5979951)

```
 sudo dnf install httpd -y
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c5cc5123-8177-4c1d-a4d5-31db9b6e9fc7)
```
sudo systemctl start httpd
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6da8e1a1-a3bb-4b44-9821-317b02501030)

5. Be sure to ensure that Apache is up and running.
```
sudo systemctl status httpd
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1a1584ff-d9c5-4ce4-85a0-a5b7258712e5)

6. Download wordpress and copy wordpress to /var/www/html
```
mkdir wordpress cd wordpress
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/02d74a7e-ba18-432c-a81c-3ffd59571b25)

```
sudo wget http://wordpress.org/latest.tar.gz sudo tar xzvf latest.tar.gz
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/25fd8dea-3e83-4ad6-b37a-cb01c5ee28aa)

```
sudo rm -rf latest.tar.gz cp wordpress/wp-config-sample.php wordpress/wp-config.php
cp -R wordpress /var/www/html/
```

7. Configure SELinux Policies
```
sudo chown -R apache:apache /var/www/html/wordpress
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/da3d0dcd-caca-4686-b6e0-1ba590c56ea9)

```
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d90d9646-acf8-4104-863d-423b2c31dd55)

```
sudo setsebool -P httpd_can_network_connect=1
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5e901929-c032-4e47-ac34-5eef7c2b78ec)

# Step 4 — Install MySQL on our DB Server EC2
**Update the system**
```
sudo yum update
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/4ba3d81f-5fc7-49d2-ac94-18cbf9c3d38a)

**Install Mysql Server**
```
sudo yum install mysql-server
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/052ab6c5-54f7-4e66-aa4a-e658090f74ed)

**Verify that the service is up and running**
```
sudo systemctl status mysqld
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/f94a9404-5ad9-4fdb-8fe5-d9293024ab74)

**if it is not running, restart the service.** 
```
sudo systemctl restart mysqld
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3853ae17-8fca-43c1-aec3-22efa5328658)

**Enable it to be running even after reboot**:
```
sudo systemctl enable mysqld
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/44519e4f-6a21-4de7-a81c-6e9dc678c3cf)
**Check status now**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c988c1fe-0b06-44ab-8dd4-bfc9c4e4ea0f)

# Step 5 - Configure DB to work with WordPress
**Log to mysql**
```
sudo mysql
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1e40b616-e837-40d0-a339-36af45d76cf2)

```
CREATE DATABASE wordpress;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7a838fda-f344-40e2-92e2-6c59a1130876)

```
CREATE USER `melkamu`@`172.31.31.188` IDENTIFIED BY 'PassWord.1';
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/f6ad9461-cf3d-4322-85f7-d3e67156daf2)

```
GRANT ALL ON wordpress.* TO 'melkamu'@'172.31.31.188';
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/60fde754-705f-4aa0-a77a-1daafdc12895)

```
FLUSH PRIVILEGES;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/4a1e4747-72de-4eda-bd1a-7f5c3ae9a1bc)


```
SHOW DATABASES;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c94b270c-dbc0-4d45-93fb-8e32cdc935a4)

```
exit
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/f9c143ce-01a0-4c24-a8d8-841c3affc400)

# Step 6 - Configure WordPress to connect to remote database
> Hint: Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, we shall allow access to the DB server ONLY from our Web Server's IP address so in the Inbound Rule configuration specify source as /32

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/023e0382-d37a-4d5b-b488-65ca6ccc4ddf)



1. Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client
```
sudo yum install mysql
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8c022e65-4e82-47cd-9ef5-1ce3ad0aa993)


**sudo mysql -u admin -p -h <DB-Server-Private-IP-address>**
```
sudo mysql -u melkamu -p -h 172.31.18.245
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7c2b9056-1cbe-4d61-a8c8-24796fcec740)


2. Verify if you can successfully connect
 ```
SHOW DATABASES;
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b260191d-a1d5-4db6-8817-5c4077a4778c)


**if sucessful the command list of existing databases.**

3. Change permissions and configuration so Apache could use WordPress:

4. Enable TCP port 80 in Inbound Rules configuration for our Web Server EC2 (enable from everywhere 0.0.0.0/0 or from our workstation's IP)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/9d070f8d-92f3-4ca7-90c0-202de8b90935)

5. Try to access from your browser the link to your WordPress
```
http://<Web-Server-Public-IP-Address>/wordpress/
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d7e13991-8b1e-4c2f-b25e-5fa25718c0d3)


![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c9ae873e-17d8-4398-a8be-1dd59c9f2869)


![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d4c719e5-5f80-41fe-baf8-9a0c6323bf10)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/9c149c3d-475b-4818-8d4e-7376224a47e4)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a23e2dd0-6eb5-427f-9a1e-a44dc0f71332)

### The End of Web Solution With WordPress project
In this project we prepared storage infrastructure on two Linux servers and implement a basic web solution using WordPress.
1. Configure storage subsystem for Web and Database servers based on Linux OS. The focus is working with disks, partitions and volumes in Linux
2. Install WordPress and connect it to a remote MySQL database server
3. Deploying Web and DB tiers of Web solution
