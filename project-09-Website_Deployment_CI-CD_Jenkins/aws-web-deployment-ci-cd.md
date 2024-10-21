# Tooling Website deployment automation with Continuous Integration
In this project we are going to start automating part of our routine tasks with a free and open source automation server - **Jenkins**. It is one of the mostl popular CI/CD tools.

Acording to Circle CI, Continuous integration (CI) is a software development strategy that increases the speed of development while ensuring the quality of the code that teams deploy. Developers continually commit code in small increments which is then automatically built and tested before it is merged with the shared repository.

**Task**
Enhance the architecture prepared in `Project 8` by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS server.

# Step 1 - Install Jenkins server
1. Create an aws EC2 instance server based on Ubuntu Server 24.04 LTS and name it `Jenkins Server`
 Log to aws account console and create EC2 instance of t2.micro type with Ubuntu Server 24.04 LTS Server launch in the default region us-east-1. name instance Linux Jenkins server
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/951d5488-5897-407e-ab66-7345ee787634)

Application and OS Images select Ubuntu free tire eligable version
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/46ec338c-7372-43c5-80a5-600615d337d8)

Create new key pair or select existing key
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0cd3dd1f-029d-48e7-bbf6-f571fdb1b73f)

Network setting create new security group or use existing security group
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5f6239a3-35f4-498b-9728-302b8f7fd8de)

Configure Storage and launch the instance
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/09520371-6a1d-4194-8498-9487ffb54268)

View Instance
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1d5ff825-a52b-40b0-ac42-b3ccb48d3ea3)

Instance Details for web
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0c1ac140-93a7-42db-b34d-f9e8729dbcd0)

Configure security group with the following inbound rules:
- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default.
- Allow traffic on port 8080  default Jenkins server uses TCP port
- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/fc376ffa-5836-4cd3-8978-25ce6d8aec9e)

2. Install JDK since Jenkins is a Java-based application
 
 Open up the Linux terminal to begin configuration
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/60042214-5b40-4a39-9e74-009e2528bfce)

```
sudo apt update
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ec3cdf07-0d63-439d-a74c-263343c0f534)

```
sudo apt install default-jdk-headless
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/795efe72-f044-4dc4-bbfa-eeb6329d8e57)

 **check if the installation worked**
```
java -version
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/3f3c9d33-74f0-422f-aa60-5cd33689e8d9)

3. Install Jenkins
Add the Jenkins repository:
```
sudo wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/bd87e0fd-e5ea-49e3-ab8c-d3adc964b491)

**Put the Jenkins repository to the list of package sources**
```
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7538e383-f182-4815-ad54-18ca3d9524e3)

```
sudo apt update
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/66df5470-262b-463c-9ca5-27f8312357aa)

**Installing and configuring Jenkins**
```
sudo apt-get install jenkins
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/cc5ebca6-29fb-4906-b53f-b34d96e1e579)

**Enable jenkins**
```
sudo systemctl enable jenkins
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/37d4625d-e37a-4fc2-8d55-38e53de07053)


**Check Jenkins is up and running**
```
sudo systemctl status jenkins
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/dc5b9848-9ff2-4484-ae70-4838a5cd1d2a)

4. By default Jenkins server uses TCP port 8080 - open it by creating a new Inbound rule in our EC2 Security Group

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ce1c01be-648d-44b6-afb9-6f14446a953b)

5. Perform initial Jenkins setup
**Perform initial Jenkins setup. From your browser access**
```
http://<Jenkins-Server-Public-IP-Address>:8080
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a2746daa-2f6e-4d75-abc4-7dce3e36b087)

**Obtain Initial Admin Password**
After accessing Jenkins through our browser via `http: <Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080` we will be prompted to provide a default admin password. T

To get retrieve the default admin password, run the following command
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5590c0f8-9b33-4122-94d0-db42bb7f6452)

Copy the password and paste 
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/dd06dc13-8568-4331-ba8b-48afaf035240)

This step enables us to authenticate as the administrator and proceed with Jenkins setup and configuration.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/91ac69f2-500f-4807-b0f9-fe0d61b05acf)

6. Install Suggested Plugins Then we will be asked which plugins to install - choose suggested plugins.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ad23b36c-e6d4-49a2-b098-ce3e4d36d315)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/2efc27eb-eb3e-47d4-bddf-2fb94bb50b70)

7. Once plugin installation is done Create the First Admin User.
we will create the first admin user for our system, granting initial administrative privileges to manage and configure the platform.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e81493cf-81e3-4526-a539-8deee4c41b3f)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1068b12a-4a8e-4769-a6c9-ca1fd4edf56b)

8. Jenkins Setup Completion.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0ecdce55-11b3-4874-899a-c2efb7e47c3b)

With the installation and configuration completed, Jenkins is now ready for use. Navigate to the Jenkins dashboard and click on `New Item` to create a new job.

# Step 2 - Configure Jenkins to retrieve source codes from GitHub using Webhooks
In this part, we will learn how to configure a simple **Jenkins job/project**. This job will will be triggered by `GitHub webhooks` and will execute a `build` task to retrieve codes from GitHub and store it locally on Jenkins server.

1. Enable webhooks in our GitHub repository settings. Go to your GitHub repository and select Settings > Webhooks > Add webhook
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/68ba7c39-aea0-42e7-bfad-23869274d86b)

2. Go to Jenkins web console, click `New Item` and create a `Freestyle project`
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/529732d8-a4d2-440b-af84-7d3ed797cf3b)

To connect our GitHub repository, we will need to provide its URL, we can copy from the repository itself
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b2bc1c1b-b62d-4f36-9f93-b2c25b72ad3d)
```
https://github.com/melkamu372/tooling.git
```
In configuration of our Jenkins freestyle project choose Git repository, provide there the link to our Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b8cd0b2a-88f0-4e83-b5d5-d158ce0cf01f)


Save the configuration and let us try to run the build. For now we can only do it manually. Click ``Build Now` button, if we have configured everything correctly, the build will be successfull and you will see it under #1
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/bf1ffc2d-663e-4615-ac8e-26ab68718e46)

You can open the build and check in **Console Output** if it has run successfully.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/8779411d-ff10-48f0-9342-78eca746b994)

But this build does not produce anything and it runs only when we trigger it manually. Let us fix it.

3. Click `Configure` our job/project and add these two configurations

3.1 Configure triggering the job from GitHub webhook:
 
 ![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/05e3efc4-4f96-4ade-bed6-819d4c531d04)

3.2 Configure `Post-build Actions` to archive all the files - files resulted from a build are called `artifacts`

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/468655cb-66d5-4bef-be8c-f10d5afe6bef)

Now, go ahead and we make some change in any file in our GitHub repository (e.g. README.MD file) and push the changes to the master branch.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c4c0cae7-4506-4aa6-81f0-68f05aaeb225)

we will see that a new build has been launched automatically by webhook and its results - artifacts, saved on Jenkins server.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/972d2d75-1f0b-403e-8150-d65557fd2fcb)

Now we configured an automated `Jenkins job ` that receives files from GitHub by webhook trigger this method is considered as `push` because the changes are being `pushed` and files transfer is initiated by GitHub. There are also other methods: trigger one job `downstreadm` from another `upstream`, pull GitHub periodically and others

**By default, the artifacts are stored on Jenkins server locally**
```
ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/66adc51b-dbb5-4b89-8056-3768e1548e8d)

# Step 3 - Configure Jenkins to copy files to NFS server via SSH

Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to `/mnt/apps` directory
Jenkins is a highly extendable application and there are `more than 1400 plugins` available. now we will need a plugin that is called `Publish Over SSH`
1. Install `Publish Over SSH` plugin. Go to the Jenkins web console and select **Manage Jenkins** > **Manage Plugins**> **Available** > **Publish over SSH** > Install without restart."
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d16f56b7-6671-438d-8f6f-c788b213aa09)

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/59524e74-719a-4003-bee7-6c687f60b6aa)

**Verify the installation**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6556d1bf-21bd-4d85-ac57-dd0f9950b444)

2. Configure the **job/project** to copy artifacts over to **NFS server**
 
 On main dashboard select **Manage Jenkins** > **Configure System** menu item.

Scroll down to `Publish over SSH` plugin configuration section and configure it to be able to connect to your NFS server:

2.1 Provide a private key (content of .pem file that we use to connect to NFS server via SSH/Putty)

2.2 Arbitrary name

2.3 Hostname - can be `private IP address` of our NFS server

2.4 Username - `ec2-user` (since NFS server is based on EC2 with RHEL 9)

2.5 Remote directory - `/mnt/apps` since our Web Servers use it as a mointing point to retrieve files from the NFS server
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c65bde96-4a6f-4d91-93ec-f2a595a768dc)

> Test the configuration and make sure the connection returns _**Success**_. **N.B** that `TCP port 22 on NFS server` must be open to receive SSH connections
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/49b59101-a825-4ad5-86ca-99a425769f30)
**Save the configuration, open your Jenkins job/project configuration page and add another one `Post-build Action`**

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0431bb34-999c-46ec-8c13-e6f4228818f3)

**Configure it to send all files produced by the build into our previouslys define remote directory** In our case we want to copy **all files** and **directories**, so we use `**` If you want to apply some particular pattern to define which files to send - [Read here in detail](https://ant.apache.org/manual/dirtasks.html#patterns).
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/546db5c7-1fd8-4036-a11e-d388b59eabff)

**Save this configuration and go ahead, change something in README.MD file in our GitHub Tooling repository**
> Remeber to give directory permissions for user `ec2-user` on the **NFS server** :
Ensure the target directory on the NFS server has the correct permissions. You might need to change ownership or modify the permissions to allow the Jenkins user to write to it.
```
sudo chown -R ec2-user:ec2-user /mnt/apps
sudo chmod -R 755 /mnt/apps
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/39399920-2d7e-4110-a6f1-7d570cbc2358)

`Webhook` will trigger a new job 
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/01332821-fb4a-47ee-b6c3-a01676e70421)

and in the `Console Output` of the job we will get something like this:

```
SSH: Transferred 25 file(s)
Finished: SUCCESS
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/91861520-8023-4c7c-80ea-1dd2333ca6b9)

**To make sure that the files in /mnt/apps have been updated - connect via SSH to our NFS server and check README.MD file**
```
sudo ls /mnt/apps
```

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c1018c18-7504-476b-8f9d-17dd1d11a2cc)


```
cat /mnt/apps/README.md
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a95bba63-6c0c-4467-806d-4a65f2bc696b)

If you see the changes you had previously made in your GitHub - the job works as expected.

### End of Tooling Website deployment automation with Continuous Integration Project
In this project we implemented our first Continous Integration solution using Jenkins. We configure a job to automatically deploy source codes changes from Git to NFS server.

