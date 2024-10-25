# Ansible Configuration Management(Automate Project 7 to 10)

In Projects 7 to 10 we had to perform a lot of manual operations to set up virtual servers, install and configure required software and deploy your web application.

This Project will make us appreciate `DevOps tools` even more by making most of the routine tasks automated with Ansible Configuration Management, at the same time we will become confident with writing code using declarative languages such as `YAML`

**Ansible Client as a Jump Server (Bastion Host)**
Ansible can be effectively used in combination with a Jump Server (also known as a Bastion Host) to manage a fleet of servers that are not directly accessible from the public internet. A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server - it provides better security and reduces attack surface.

On the diagram below the Virtual Private Network (VPC) is divided into two subnets - Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.



![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a324426f-c7f1-42dc-bb6e-4a721ca39cf1)



When you get to Project 15, we will see a Bastion host in proper action. But for now, we will develop **Ansible scripts** to simulate the use of a Jump box/Bastion host to access our Web Servers

**Tasks**

1. Install and configure Ansible client to act as **a Jump Server/Bastion** Host

2. Create a simple **Ansible playbook** to automate servers configuration


# Step 1 -Install and Configure ANSIBLE ON EC2 Instance

1. Update Name tag on our **Jenkins EC2** Instance to **Jenkins-Ansible**. We will use this server to run playbooks.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/848adc8b-31f7-41b9-a6c3-45bd52d74d15)


2. In your GitHub account create a new repository and name it **ansible-config-mgt**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/88e8764a-0754-4693-bb5b-4a3eff8dfdbf)

**Created Repository is**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/dfa4b1e1-91c4-4d87-801b-c5894697524c)

3. In your `Jenkin-Ansible server`, instal Ansible

```
sudo apt update
```
```
sudo apt install ansible
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/66e43ff4-ce20-471c-946e-5ef6159a14c1)

**Check your Ansible version** 
```
ansible --version
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/10d49174-d515-4570-82ef-112d889f3c19)

4. Configure Jenkins build job to save our repository content every time you change it – this will solidify your Jenkins configuration skills acquired in Project 9

- Configure Webhook in GitHub and set webhook to trigger ansible build
  Enable webhooks in our GitHub repository settings. Go to your GitHub repository and select Settings > Webhooks > Add webhook

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ac88604e-24e8-4663-8604-5cf25f219733)

- Create a new Freestyle project ansible in Jenkins
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c7c92db6-9e6d-4336-adf4-76847023e386)

Point  to your `ansible-config-mgt` repository

Get your repository address  fromhere
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d2f7c94d-c685-4854-a729-a36927835b7e)

```
https://github.com/melkamu372/ansible-config-mgt.git
```
 
 Paste address to git configureation

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5a0c4c60-169f-4d7f-89d0-47e38eeeaca3)

- Configure a Post-build job to save all (**) files, like you did it in _Project 9_.

**Configure triggering the job from GitHub webhook**:
  
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ab12fbcd-90ee-447c-abba-93ed2fec8739)

**Configure Post-build Actions to archive all the files - files resulted from a build are called artifacts**

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/977205d0-6194-4ee4-80f1-bb8f923b5e7f)

5. Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/62ef1cfb-bd41-4e8e-8030-8d5f9737dd1c)

**Console**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7bc3cbad-40c1-497d-8da3-7bd93b1a9216)


```
ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d4c3d190-b3f2-4b56-bbd7-87f4aca800b8)


> Note: Trigger Jenkins project execution only for /main (master) branch.

Now your setup will look like this:

![6035](https://user-images.githubusercontent.com/85270361/210153832-75e74f67-0654-4fc1-bcdd-08c3d5a8fa76.PNG)

> Tip Every time you stop/start your Jenkins-Ansible server – you have to reconfigure GitHub webhook to a new IP address, in order to avoid it, it makes sense to allocate an Elastic IP to your Jenkins-Ansible server (you have done it before to your LB server in 
Project 10). Note that Elastic IP is free only when it is being allocated to an EC2 Instance, so do not forget to release Elastic IP once you terminate your EC2 Instance.

# Step 2 – Prepare your development environment using Visual Studio Code

1. First part of `DevOps` is ``Dev``, which means you will require to write some codes and you shall have proper tools that will make your coding and debugging comfortable – you need an Integrated development environment (IDE) or Source-code Editor. There is a 
plethora of different IDEs and Source-code Editors for different languages with their own advantages and drawbacks, you can choose whichever you are comfortable with, but we recommend one free and universal editor that will fully satisfy your needs – 
**Visual Studio Code (VSC)**

2. After you have successfully installed ``VSC``, configure it to connect to your newly created GitHub repository.

3. Clone down(Download) your ansible-config-mgt repo to your Jenkins-Ansible instance `git clone <ansible-config-mgt repo link>`

```
git clone https://github.com/melkamu372/ansible-config-mgt.git
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/33ace95e-fe63-481e-927e-57652dbecdef)


# Step 3 - Begin Ansible Development
In your ``ansible-config-mgt`` GitHub repository, 

1. create a new branch that will be used for development of a new feature

> Tip: Give your branches descriptive and comprehensive names, for example, if you use `Jira` or `Trello` as a project management tool - include ticket number (e.g. PRJ-num) in the name of your branch and add a topic and a brief description what this branch is about - a bugfix, hotfix, feature, release (e.g. feature/prj-145-lvm)

**Navigate to your repository directory:**
```
cd path/to/your/ansible-config-mgt

```
**Create a new branch:**
```
git checkout -b feature/prj-11-ansible-setup
```

2. Checkout the newly created feature branch to your local machine and start building your code and directory structure
```
git fetch
git checkout feature/prj-11-ansible-setup
```

3. Create a directory and name it **playbooks** - it will be used to store all your playbook files.
```
mkdir playbooks
```
4. Create a directory and name it **inventory** - it will be used to keep your hosts organised
```
mkdir inventory
```
5. Within the playbooks folder, create your first playbook, and name it ``common.yml``
```
touch playbooks/common.yml
```

6. Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging, Testing and Production) dev, staging, uat, and prod respectively. 
```
touch inventory/dev.yml
touch inventory/staging.yml
touch inventory/uat.yml
touch inventory/prod.yml
```
These inventory files use ``.ini`` languages style to configure Ansible hosts.

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/207c9dcb-a39d-46e4-b36b-14401a240098)

# Step 4 - Set up an Ansible Inventory
An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an **Inventory**

Save the below inventory structure in the ``inventory/dev`` file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

> Note: Ansible uses TCP ``port 22`` by default, which means it needs to ssh into target servers from Jenkins-Ansible host - for this you can implement the concept of ``ssh-agent``. Now you need to import your key into ssh-agent:

> To learn how to setup SSH agent and connect VS Code to your Jenkins-Ansible instance, please see this video:

- For Windows users - [ssh-agent on windows](https://www.youtube.com/watch?v=OplGrY74qog) 
- For Linux users - [ssh-agent on linux](https://www.youtube.com/watch?v=OplGrY74qog)

**Start the SSH agent:**
```
eval `ssh-agent -s`
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e47060d4-eeb9-4bc2-81ae-427a9225d732)


**Add your private key to the SSH agent**: ssh-add <path-to-private-key> Replace <path-to-private-key> with the actual path to your private key file

```
ssh-add melkamu_key.pem
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/09800ded-b2a4-4a04-9aec-6538de66be3c)


Confirm the key has been added with the command below, you should see the name of your key

```
ssh-add -l 
```
ssh![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/7b1e7ad0-f18a-4e50-8a48-b660e86b7b5a)

Now, ssh into your Jenkins-Ansible server using ssh-agent
```
ssh -A ubuntu@34.234.86.105
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/030be498-be21-48ac-a35c-6eee4ef27a7a)

> To learn how to setup SSH agent and connect VS Code to your Jenkins-Ansible instance,    please see this video: [Windows Linux](https://www.youtube.com/watch?v=OplGrY74qog)

> Also notice, that your Load Balancer user is ``ubuntu`` and user for RHEL-based servers is ``ec2-user``

Update your ``inventory/dev.yml`` file with this snippet of code:

[nfs]
```
<NFS-Server-Private-IP-Address> ansible_ssh_user=ec2-user
```
[nfs]
```
<NFS-Server-Private-IP-Address> ansible_ssh_user=ec2-user
```
[webservers]
```
<Web-Server1-Private-IP-Address> ansible_ssh_user=ec2-user
<Web-Server2-Private-IP-Address> ansible_ssh_user=ec2-user
```
[db]
```
<Database-Private-IP-Address> ansible_ssh_user=ec2-user 
```
[lb]
```
<Load-Balancer-Private-IP-Address> ansible_ssh_user=ubuntu
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/208497c4-aea9-4bf9-989b-8bcdc8da0841)

# Step 5 - Create a Common Playbook

It is time to start giving Ansible the instructions on what you need to be performed on all servers listed in `inventory/dev`

In `common.yml` playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

**Update your playbooks/common.yml file with following code**

```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  become: yes
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest
   

- name: update LB server
  hosts: lb
  become: yes
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e8eaf2f3-0de3-4b17-ae01-e1c26b4032ca)

> Examine the code above and try to make sense out of it.  This **playbook** is divided into two parts, each of them is intended to perform the same task :

 `install wireshark`  utility or make sure it is updated to the latest version on your RHEL 8 and Ubuntu servers. 
  
It uses root user to perform this task and respective package manager: `yum` for **RHEL 8** and `apt` for **Ubuntu**

**Feel free to update this playbook with following tasks:**

- Create a directory and a file inside it

- Change timezone on all servers

- Run some shell script

For a better understanding of Ansible playbooks - watch this video from RedHat [Introduction to Ansible Playbooks (and demonstration)](https://www.youtube.com/watch?v=ZAdJ7CdN7DY) and read this article [What is an Ansible Playbook?](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook)


# Step 6 - Update GIT with the latest code

Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

> In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with help of GIT. In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes - it is also called ``Four eyes principle``.

Now you have a separate branch, you will need to know how to raise a **Pull Request** (PR), get your branch peer reviewed and merged to the master branch.

**Commit your code into GitHub:**

1. Use git commands to add, commit and push your branch to GitHub.
```
git status
```
```
git add <selected files>
```
```
git commit -m "commit message"
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/1b91aa7a-a6be-4f55-b6fd-fe7cb08bbeff)
**Then push to github**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/fbdc6f52-5465-449f-b46e-256b5584de93)

2. Create a Pull Request (PR)
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/259d4361-f2de-4a56-bead-9b119655ae53)

3. Wear the hat of another developer for a second, and act as a reviewer.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/e0d5fb1e-007b-4ccf-ad72-74701325560b)

4. If the reviewer is happy with your new feature development, merge the code to the master branch.
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/ebc11c68-2471-4b4b-91ba-8d7e97cb1fdb)

5. Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes
   ![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/39a7e9a9-bbdd-4b1e-9b09-41f29656f2ae)

Once your code changes appear in master branch - Jenkins will do its job and save all the files (build artifacts) to 

`/var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/`

directory on Jenkins-Ansible server.

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5fae49b1-785f-4cf9-ba59-61c44a735d6a)

```
ls /var/lib/jenkins/jobs/ansible/builds/12/archive/
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/0929f100-06cb-4f2f-b0b0-cafd71377076)


# Step 7 - Run first Ansible test

Now, it is time to execute ansible-playbook command and verify if your playbook actually works: first setup our vs code  to connect our instance for remote development, follow these steps:

1. **Install Remote - SSH Extension**

Open VS Code -> Go to the Extensions view by clicking on the Extensions icon in the Activity Bar on the side of the VS Code window -> Search for `Remote - SSH` extension and install it
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/52f9bff3-bf47-42a3-a34a-096bd3b99550)

2. Configure SSH Host

Open the Command Palette (Ctrl+Shift+P or Cmd+Shift+P on macOS)->Type and select Remote-SSH: Connect to Host....
->Enter the SSH connection string in the format user@hostname or user@ip_address, 


![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/a4c7d027-dbaa-469a-bb36-558f246120e3)
```
ubuntu@34.234.86.105
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/6582d210-5a6e-477f-a3af-cde247029fc8)

VS Code will prompt you for your SSH password or passphrase.

Connect to Instance
After entering the password or passphrase, VS Code will connect to your remote instance via SSH.
You should see a new VS Code window open with a green indicator in the bottom-left corner, indicating the remote connection.
Edit Files and Execute Commands
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/299436d9-85fb-45f2-8c36-4fb10cc5f91e)

You can now edit files directly on your remote instance using VS Code.
Open your Ansible playbook and other project files to make changes or additions as needed.
Use the integrated terminal in VS Code to run commands on the remote instance directly from the editor.
```
cd ansible-config-mgt
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/c5ed53b5-f187-4f0a-a94a-0dd2957b3764)

```
ansible-playbook -i inventory/dev.yml playbooks/common.yml
```
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/d7e4414a-0c0d-4e58-9767-0a1d649588c7)

You can go to each of the servers and check if wireshark has been installed by running 
```
which wireshark
```
 **or**
 ```
 wireshark --version
```
**Server check Ubuntu server**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/35372206-3e55-49f7-a24f-a7f954090df5)

**Another Server Redhate server**
![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/5ee2b263-c6a9-4539-92ce-4968461b124a)


Your updated with Ansible architecture now looks like this:

![image](https://github.com/melkamu372/StegHub-DevOps-Cloud-Engineering/assets/47281626/b9983c20-fc00-4ad1-86b8-c41677ce629d)


# Optional step - Repeat once again

Update your ansible playbook with some new Ansible tasks and go through the full 
**checkout** -> **change codes**->**commit** -> **PR** -> **merge** -> **build** -> **ansible-playbook** cycle again to see how easily you can manage a 
servers fleet of any size with just one command!

### The end of Project 11 Ansible Configuration Management(Automate Project 7 to 10)
In this project we develop **Ansible scripts** to simulate the use of a **Jump box/Bastion host** to access our Web Servers to achive this we do that 
 
- Install and configure Ansible: We set up Ansible on a Jenkins EC2 instance, transforming it into a Jump Server/Bastion Host for secure management of internal servers.

- Develop Ansible Playbooks: We created and organized Ansible playbooks and inventory files to automate the configuration of various servers in our environment.

- Integrate with Jenkins and GitHub: We configured Jenkins to build and archive our Ansible configuration files automatically from a GitHub repository, streamlining our DevOps workflow.



