
## Disk Management in Linux: Detailed Notes

**Disk management** in Linux involves creating, modifying, and managing partitions, file systems, and disk quotas. It's an essential aspect of system administration to ensure efficient utilization and organization of disk space. Below is a comprehensive guide on disk management in Linux.
### 1. Understanding Disks, Partitions, and File Systems

- **Disks:** Physical storage devices like hard drives (HDD), solid-state drives (SSD), USB drives, etc. They are represented as `/dev/sda`, `/dev/sdb`, etc.

- **Partitions:** Logical divisions within a disk that act as separate storage units. A single disk can have multiple partitions.
  - **Primary Partition:** A disk can have up to 4 primary partitions.
  - **Extended Partition:** Used to overcome the 4-partition limit by acting as a container for multiple logical partitions.
  - **Logical Partitions:** Partitions inside the extended partition.

- **File Systems:** Determine how data is stored and organized on the disk. Common file systems in Linux include:

  - `ext4`: Default file system for most Linux distributions.
  - `xfs`: Scalable and robust, often used for large systems.
  - `btrfs`: Advanced, with snapshot and RAID support.
  - `vfat`: Used for compatibility with Windows and DOS.
  - `swap`: Used for memory swapping.

### 2. Listing and Viewing Disk Information
- `lsblk` (List Block Devices): Displays block devices (disks and partitions) in a tree structure

```
lsblk

```
- `fdisk -l`: Lists all disk partitions and their sizes, types, and other details

```
sudo fdisk -l
```
- `df -h`: Shows mounted file systems and their usage in human-readable form.

```
df -h
```
- `du -sh`: Displays disk usage of directories/files.

```
du -sh /path/to/directory
```

### 3. Disk Partitioning

- `fdisk`: A command-line tool for creating and managing partitions. It is used for MBR (Master Boot Record) partitioning.
  - Open fdisk on a disk (e.g., /dev/sda):

  ```
  sudo fdisk /dev/sda
  ```
  - Common commands inside fdisk
    - `n`: Create a new partition.
    - `p`: Print the partition table.
    - `d`: Delete a partition.
    - `w`: Write changes and exit.
    - `q`: Quit without saving changes.

- `parted`: Used for more advanced partitioning (supports both MBR and GPT partitioning)
  - Example usage:

  ```
  sudo parted /dev/sda
  ```
  - Important commands inside parted:

    - `mklabel gpt`: Create a new GPT partition table.
    - `mkpart primary ext4 1MiB 100%`: Create a new primary partition from 1 MiB to the end of the disk.
    - `print`: Display partition table.
    - `resizepart`: Resize a partition.

### 4. Formatting Partitions

After creating partitions, they need to be formatted with a file system. Common formatting commands:
- `mkfs.ext4`: Format a partition with the ext4 file system

```
sudo mkfs.ext4 /dev/sda1
```
- `mkfs.xfs`: Format a partition with the xfs file system

```
sudo mkfs.xfs /dev/sda1
```

- `mkswap`: Format a partition for swap

```
sudo mkswap /dev/sda2
```
- `mkfs.vfat`: Format with the FAT32 file system for compatibility with Windows

```
sudo mkfs.vfat /dev/sda1
```

### 5. Mounting and Unmounting File Systems

- **Mounting**: Making a file system accessible at a specific directory in Linux (known as the mount point).

```
sudo mount /dev/sda1 /mnt/mydisk
```
To mount the file system at boot, you can add an entry to `/etc/fstab`:

```
/dev/sda1   /mnt/mydisk   ext4   defaults   0 2
```

- Unmounting: Removing access to a mounted file system

```
sudo umount /mnt/mydisk
```

- Listing Mounted File Systems

```
mount | grep "^/dev"
```

### 6. Swap Management

Swap space is used when physical RAM is full. Linux will move inactive pages to the swap space.

- Creating Swap Space:
  - Create a swap partition or a swap file.

  ```
  sudo dd if=/dev/zero of=/swapfile bs=1G count=4
  sudo mkswap /swapfile
  sudo swapon /swapfile
  ```
- Enable Swap on Boot: Add the swap entry in `/etc/fstab`:

```
/swapfile   none   swap   sw   0 0
```
- Monitoring Swap Usage:

```
swapon --show
free -h
```

### 7. Disk Quotas

Disk quotas are used to limit the amount of disk space or the number of files a user can use.

- **Installing Quota Tools**: On Debian/Ubuntu

```
sudo apt install quota
```
On RHEL/CentOS:

```
sudo yum install quota
```

- Enable Quotas on File Systems:

  - Edit `/etc/fstab` to add the `usrquota` and `grpquota` options for the file system:

  ```
  /dev/sda1   /   ext4   defaults,usrquota,grpquota   0 1
  ```
  - Remount the file system:

  ```
  sudo mount -o remount /
  ```

- Initialize Quotas:

```
sudo quotacheck -cum /
sudo quotaon /
```

- Assign Quotas to Users:

```
sudo edquota -u username
```
This opens an editor where you can set soft and hard limits for disk space and file limits.


### 8. LVM (Logical Volume Manager)

LVM allows flexible disk management by creating logical volumes that can be resized easily.

- **LVM** Components:
  - **Physical Volumes (PV)**: Physical disks or partitions.
  - **Volume Groups (VG)**: A pool of physical volumes.
  - **Logical Volumes (LV)**: Allocated from volume groups and used as virtual partitions.
- **LVM Workflow:**

  **1. Create Physical Volume:**

  ```
  sudo pvcreate /dev/sda1
  ```

  **2. Create Volume Group:**

  ```
   sudo vgcreate my_vg /dev/sda1
  ```

  **3. Create Logical Volume:**

  ```
  sudo lvcreate -L 10G -n my_lv my_vg
  ```

  **4. Format and Mount Logical Volume:**

- **Resize Logical Volume:**
  - To extend an existing logical volume

  ```

  sudo lvextend -L +5G /dev/my_vg/my_lv
  sudo resize2fs /dev/my_vg/my_lv
  ```

### 9. Monitoring Disk Usage

- `df`: Reports file system disk space usage

```
df -h
```

- `du`: Shows disk usage of files and directories.

```
du -sh /path/to/directory
```

- `iostat`: Displays disk I/O statistics.

```
iostat -x
```
### 10. Troubleshooting and Maintenance

`fsck`: Check and repair file system integrity.

```
sudo fsck /dev/sda1
```

- `badblocks`: Scan a disk for bad sectors.

```
sudo badblocks -v /dev/sda
```
Creating Linux partitions with `fdisk` involves the following steps. Be careful when using `fdisk`, as it can modify the partition table, potentially leading to data loss if not used properly.

### Steps to Create Linux Partitions Using `fdisk`:

**1. Open a Terminal:** Press Ctrl+Alt+T or open a terminal window in your Linux environment.

**2. List Disks:** Before starting, identify the disk where you want to create partitions by listing all available disks.

```
sudo fdisk -l
```
This will display information about your disks, e.g., `/dev/sda`, `/dev/sdb`.

**3. Run fdisk on the Target Disk:** Choose the disk you want to partition, e.g., /dev/sda. Replace /dev/sda with your actual disk.

```
sudo fdisk /dev/sda
```
**4. View Current Partitions (Optional):** Once inside fdisk, you can type p to view existing partitions on the disk.

**5. Create a New Partition:**

- To create a new partition, press n.
- Choose between creating a **primary** or **extended** partition. You can create up to 4 primary partitions. Press `p`for primary or `e` for extended.
- It will ask you for **a partition number** (1-4), the **first sector** (press `Enter` to choose the default), and the **last sector** or the size of the partition (e.g., `+2G` for a 2 GB partition). You can type a size in `+sizeM` or `+sizeG` format for MB or GB.

**6. Write the Changes:** After creating the partition, you can write the changes to the disk by typing **`w`**. This saves the partition table and exits **`fdisk`**.

**7. Format the Partition:** After creating the partition, it needs to be formatted. For example, to format it with the `ext4` file system:

```
sudo mkfs.ext4 /dev/sda1
```
Replace `/dev/sda1` with your actual partition.

**8. Mount the Partition (Optional):** If you want to use the partition immediately, you can mount it to a directory:

```
sudo mkdir /mnt/mydisk
sudo mount /dev/sda1 /mnt/mydisk

```
**Example Walkthrough:**

1. Open fdisk for the disk `/dev/sda`:

```
sudo fdisk /dev/sda
```

2. Create a new partition:
- Press `n`, choose `p` (for primary), and set the partition number to `1`.
- Use default values for the first sector by pressing `Enter`.
- Set the last sector size as `+2G` for a 2 GB partition.

3. Write the changes with `w`.

4. Format the new partition with `ext4`:

```
sudo mkfs.ext4 /dev/sda1
```
Now you've successfully created and formatted a Linux partition using `fdisk`.

**Conclusion**

Managing disks in Linux involves a variety of tools and techniques, from partitioning and formatting to mounting and monitoring disk usage. Understanding these tools will help system administrators efficiently manage storage resources. Always remember to back up data before making major changes to disk partitions or file systems!


## The Challenges and Solutions
- I face Challenges when installing  `php` . But resolved by following the following steps:

1. Check for Existing Swap File:

```
sudo swapon --show
```

2. Turn Off Existing Swap File:

```
sudo swapoff /swapfile
```

3. Remove the Existing Swap File: 

```
sudo rm /swapfile
```
4. Create a New Swap File: 

```
sudo fallocate -l 1G /swapfile
```
Or, if you prefer to use dd, create a smaller swap file to avoid memory issues:

```
sudo dd if=/dev/zero of=/swapfile bs=1M count=512
```
5. Set Permissions and Format the Swap File:

```
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

6. Verify Swap File Activation: 

```
sudo swapon --show
```
7. Make Swap Permanent (Optional): 

```
echo '/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab
```
  

- I have Got php installation Conflict and  I resolved by upgrading from :
`sudo yum  install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm` to `sudo dnf install -y  https://rpms.remirepo.net/enterprise/remi-release-9.rpm`

