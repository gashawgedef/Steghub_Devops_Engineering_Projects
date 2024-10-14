
# Self Study, Challenges and  Solutions

## Network-attached storage (NAS), Storage Area Network (SAN)  NFS, (s)FTP, SMB, iSCSI. 
> Now let getstaered by breake down each conceptes one by one 

## Storage Technologies and Protocols

### Network-attached Storage (NAS)
**NAS** is a storage device connected to a network that allows data to be accessed by multiple clients. It operates at the file level, providing shared storage that can be accessed over a network, typically using protocols like NFS (Network File System), SMB (Server Message Block), or FTP (File Transfer Protocol).

### Storage Area Network (SAN)
**SAN** is a high-speed network that provides access to block-level storage. Unlike NAS, SANs operate at the block level, which means they deal with raw storage blocks rather than files. Common protocols used in SANs include Fibre Channel, iSCSI (Internet Small Computer Systems Interface), and Fibre Channel over Ethernet (FCoE).

### Related Protocols
1. **NFS (Network File System):** A protocol used for distributed file systems, allowing a user on a client computer to access files over a network similar to local storage.
2. **(S)FTP (File Transfer Protocol / Secure File Transfer Protocol):** Protocols for transferring files over a network. SFTP is the secure version that provides encryption.
3. **SMB (Server Message Block):** A protocol for network file sharing, primarily used by Windows systems.
4. **iSCSI (Internet Small Computer Systems Interface):** A protocol for linking data storage facilities over IP networks. It enables the creation of SANs using existing network infrastructure.

### Block-level Storage
Block-level storage refers to storage that is managed in blocks, which are the smallest units of storage. Each block acts like an individual hard drive, and the storage is typically used for databases, virtual machine file systems, and other high-performance applications. Cloud service providers often use block-level storage for services that require low latency and high IOPS (Input/Output Operations Per Second).

### Object Storage vs. Block Storage
- **Block Storage:** Manages data in fixed-sized blocks. It’s used for applications requiring fast, consistent I/O performance. Each block can be treated independently, so changes in one block do not affect others. Commonly used for databases, VMs, and other transactional data.
- **Object Storage:** Manages data as objects, each containing the data itself, metadata, and a unique identifier. It’s optimized for massive amounts of unstructured data, such as media files, backups, and logs. Each object is stored in a flat address space, making it highly scalable and durable but not ideal for transactional data that requires frequent updates.

## AWS Storage Services
1. **Amazon Elastic Block Store (EBS):** Provides block-level storage volumes for use with Amazon EC2 instances. EBS is designed for workloads that require consistent, low-latency performance, such as databases and applications with frequent read/write operations.
2. **Amazon Simple Storage Service (S3):** An object storage service that provides scalable, durable storage for a wide variety of data, including backups, archives, and big data. S3 is ideal for storing unstructured data that is accessed infrequently.
3. **Amazon Elastic File System (EFS):** A network file system that provides scalable file storage for use with AWS Cloud services and on-premises resources. EFS supports the NFS protocol, making it suitable for workloads that require shared file access across multiple instances.

## Key Differences in AWS Context
- **EBS (Block Storage):** Suitable for low-latency, high-performance applications such as databases and enterprise applications. Provides granular control over the storage blocks and allows for consistent performance tuning.
- **S3 (Object Storage):** Ideal for storing large amounts of unstructured data. It's designed for high durability, scalability, and accessibility but is not optimized for high-performance transactional workloads.
- **EFS (Network File System):** Provides file-level storage accessible across multiple instances. It supports the NFS protocol and is ideal for applications that require shared access to files with consistent performance.


## The Challenges and Solutions

### I face Challenges when installing  `php` . But resolved by following the following steps:

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
  

### I have Got php installation Conflict and  I resolved by upgrading from :
`sudo yum  install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm` to `sudo dnf install -y  https://rpms.remirepo.net/enterprise/remi-release-9.rpm`



## Conclusion

This project not only deepened my understanding of client-server architecture but also provided valuable hands-on experience in implementing a DevOps tooling website using AWS. Configuring multiple web servers to connect with a central database server and managing shared storage through Network File System (NFS) were critical components in ensuring the reliability and performance of our infrastructure. Each aspect of this setup, from server deployment to file synchronization, played an essential role in achieving a stable and efficient environment that enhances our DevOps operations.

## Continuous Learning

This experience highlighted the importance of continuous learning, especially in cloud-based infrastructure like AWS. As cloud technologies and DevOps practices evolve, staying updated with best practices and new features is crucial for optimizing solutions. Engaging with various AWS services and tools throughout this project has reinforced my commitment to further expand my knowledge in cloud infrastructure and DevOps methodologies. This ongoing learning is essential to navigate the ever-changing technological landscape and to deliver effective and innovative solutions in the future.