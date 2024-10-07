
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