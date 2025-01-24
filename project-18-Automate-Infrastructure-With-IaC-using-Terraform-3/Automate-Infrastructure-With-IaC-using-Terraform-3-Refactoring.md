# AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM PART 3 – REFACTORING

In two previous projects we have developed AWS Infrastructure code using Terraform and tried to run it from your local workstation.
Now it is time to introduce some more advanced concepts and enhance our code.

Firstly, we will explore alternative Terraform 
[backends](https://developer.hashicorp.com/terraform/language/settings/backends/configuration).


## Introducing Backend on S3
Each Terraform configuration can specify a backend, which defines where and how operations are performed, where state snapshots are 
stored, etc.Take a peek into what the states file looks like. It is basically where terraform stores all the state of the infrastructure in json
format.So far, we have been using the default backend, which is the local backend – it requires no configuration, and the states file is
stored locally. This mode can be suitable for learning purposes, but it is not a robust solution, so it is better to store it in some
more reliable and durable storage.The second problem with storing this file locally is that, in a team of multiple DevOps engineers, other engineers will not have
access to a state file stored locally on your computer.To solve this, we will need to configure a backend where the state file can be accessed remotely other DevOps team members. There areplenty of different standard backends supported by Terraform that you can choose from. Since we are already using AWS – we can 
choose an [S3 bucket as a backend](https://developer.hashicorp.com/terraform/language/settings/backends/s3).

Another useful option that is supported by S3 backend is State Locking – it is used to lock your state for all operations that could
write state. This prevents others from acquiring the lock and potentially corrupting your state. 
[State Locking](https://developer.hashicorp.com/terraform/language/state/locking) feature for S3 backend is optional and requires 
another AWS service – DynamoDB.

Let us configure it! Here is our plan to Re-initialize Terraform to use S3 backend:

- Add S3 and DynamoDB resource blocks before deleting the local state file
- Update terraform block to introduce backend and locking
- Re-initialize terraform
- Delete the local tfstate file and check the one in S3 bucket
- Add outputs
- terraform apply


To get to know how lock in DynamoDB works, read the following [article](https://angelo-malatacca83.medium.com/aws-terraform-s3-and-dynamodb-backend-3b28431a76c1)


Create a file and name it `backend.tf` Add the below code and replace the name of the S3 bucket you created in Project-16.
```
resource "aws_s3_bucket" "terraform_state" {
  bucket = "dev-terraform-bucket"
  # Enable versioning so we can see the full revision history of our state files
  versioning {
    enabled = true
  }
  # Enable server-side encryption by default
  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }
}
```
![image](assets/pr18-001-backend-1.jpg)

You must be aware that Terraform stores secret data inside the state files. Passwords, and secret keys processed by resources are 
always stored in there. Hence, you must consider to always enable encryption. You can see how we achieved that with 
server_side_encryption_configuration.

Next, we will create a DynamoDB table to handle locks and perform consistency checks. In previous projects, locks were handled with
a local file as shown in terraform.tfstate.lock.info. Since we now have a team mindset, causing us to configure S3 as our backend
to store state file, we will do the same to handle locking. Therefore, with a cloud storage database like DynamoDB, anyone running
Terraform against the same infrastructure can use a central location to control a situation where Terraform is running at the same 
time from multiple different people.

- Dynamo DB resource for locking and consistency checking:

```
resource "aws_dynamodb_table" "terraform_locks" {
  name         = "terraform-locks"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "LockID"
  attribute {
    name = "LockID"
    type = "S"
  }
}
```
![image](assets/pr18-001-backend-2.jpg)

Terraform expects that both S3 bucket and DynamoDB resources are already created before we configure the backend. So, let us run
terraform apply to provision resources.

- Configure S3 Backend

```
terraform {
  backend "s3" {
    bucket         = "gashaw-dev-terraform-bucket"
    key            = "global/s3/terraform.tfstate"
    region         = "eu-central-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

![image](https://github.com/user-attachments/assets/9da4b56f-2e8d-4348-827d-412479318533)
![image](https://github.com/user-attachments/assets/99519c6f-de8f-4e72-a5b8-63be117abed7)

Now its time to re-initialize the backend. Run terraform init and confirm you are happy to change the backend by typing yes
```
terraform init
```
![image](https://github.com/user-attachments/assets/ec4e71c1-701b-429d-8d84-902277cd11d6)

-  Verify the changes
Before doing anything if you opened AWS now to see what happened you should be able to see the following:

- tfstatefile is now inside the S3 bucket

- DynamoDB table which we create has an entry which includes state file status

Navigate to the DynamoDB table inside AWS and leave the page open in your browser.
Run terraform plan and while that is running, 

```
terraform plan
```
refresh the browser and see how the lock is being handled:


![image](assets/pr18-005-backemd-5.jpg)




5. Add Terraform Output
Before you run terraform apply let us add an output so that the S3 bucket Amazon Resource Names ARN and DynamoDB table name can be
displayed.

Create a new file and name it output.tf and add below code

```
output "s3_bucket_arn" {
  value       = aws_s3_bucket.terraform_state.arn
  description = "The ARN of the S3 bucket"
}
output "dynamodb_table_name" {
  value       = aws_dynamodb_table.terraform_locks.name
  description = "The name of the DynamoDB table"
}
```
![image](assets/pr18-006-backemd-6.jpg)


Now we have everything ready to go!

## 6. Let us run terraform apply
Terraform will automatically read the latest state from the S3 bucket to determine the current state of the infrastructure. Even if 
another engineer has applied changes, the state file will always be up to date.

Now, head over to the S3 console again, refresh the page, and click the grey “Show” button next to “Versions.” You should now see 
several versions of your terraform.tfstate file in the S3 bucket:
```
terraform apply
```
![image](assets/pr18-007-backemd-7.jpg)

![image](assets/pr18-008-backemd-8.jpg)


With help of remote backend and locking configuration that we have just configured, **collaboration is no longer a problem**.

However, there is _still one more problem_: Isolation Of Environments. Most likely we will need to create resources for different 
environments, such as: Dev, sit, uat, preprod, prod, etc.

This separation of environments can be achieved using one of two methods:

a. [Terraform Workspaces](https://developer.hashicorp.com/terraform/language/state/workspaces)

b. Directory based separation using terraform.tfvars file



## WHEN TO USE WORKSPACES OR DIRECTORY?
To separate environments with significant configuration differences, use a directory structure. Use workspaces for environments that
do not greatly deviate from each other, to avoid duplication of your configurations. Try both methods in the sections below to help 
you understand which will serve your infrastructure best. 

For now, you can read more about both alternatives 
[here](https://developer.hashicorp.com/terraform/tutorials/modules/organize-configuration) and try both methods yourself, but we will 
explore them better in **next projects**
**Create Environment-Specific Variable Files**
In our root directory create separate `.tfvars` files for each environment to specify the variable values. For example:

- `dev.tfvars`
- `1sit.tfvars`
- `uat.tfvars`
- `preprod.tfvars`
- `prod.tfvars`
  
 **Then Create workspaces for each environment**
```
terraform workspace new dev
terraform workspace new sit
terraform workspace new uat
terraform workspace new preprod
terraform workspace new prod
```
**Create workspaces for each environment**

```
terraform workspace new dev
terraform workspace new sit
terraform workspace new uat
terraform workspace new preprod
terraform workspace new prod
```
**Switch Workspace and Apply Configuration**
When you want to apply configurations to a specific environment, switch to the corresponding workspace and use the appropriate variable file.
For example, to deploy to the dev environment:
```
terraform workspace select dev
terraform plan -var-file=dev.tfvars
terraform apply -var-file=dev.tfvars
```
> **It is enough as an introduction; we will explore them better in the next projects.**

**Security Groups refactoring with dynamic block**

For repetitive blocks of code you can use [dynamic blocks](https://developer.hashicorp.com/terraform/language/expressions/dynamic-blocks) 
in Terraform, to get to know more how to use them – [watch this video](https://youtu.be/tL58Qt-RGHY).

**Refactor Security Groups create all security groups dynamically**
![image](https://github.com/user-attachments/assets/95b09e2c-50b8-46ea-9aba-77c386320223)

**creating dynamic ingress security groups**
![image](https://github.com/user-attachments/assets/959433c5-4569-4868-9f32-9ef29f594819)


**EC2 refactoring with Map and Lookup**
Remember, every piece of work you do, always try to make it dynamic to accommodate future changes. Amazon Machine Image (AMI) is a 
regional service which means it is only available in the region it was created. But what if we change the region later, and want to
dynamically pick up AMI IDs based on the available AMIs in that region? This is where we will introduce Map and Lookup functions.

**Map uses a key and value pairs as a data structure that can be set as a default type for variables.**
```
variable "images" {
    type = "map"
    default = {
        us-east-1 = "image-1234"
        us-west-2 = "image-23834"
    }
}
```
![image](https://github.com/user-attachments/assets/669a0871-b611-44a2-ba9e-e8a6f3ca4751)

To select an appropriate AMI per region, we will use a lookup function which has following syntax: lookup(map, key, [default]).

Note: A default value is better to be used to avoid failure whenever the map data has no key.

```
resource "aws_instace" "web" {
    ami  = "${lookup(var.images, var.region), "ami-12323"}
   }
```
![image](https://github.com/user-attachments/assets/1b2c4acb-630c-4b19-96e9-69a0c90fe52c)

Now, the lookup function will load the variable images using the first parameter. But it also needs to know which of the key-value 
pairs to use. That is where the second parameter comes in. The key us-east-1 could be specified, but then we will not be doing 
anything dynamic there, but if we specify the variable for region, it simply resolves to one of the keys. That is why we have used
var.region in the second parameter.

**Conditional Expressions**
If you want to make some decision and choose some resource based on a condition – you shall use
[Terraform Conditional Expressions](https://developer.hashicorp.com/terraform/language/expressions/conditionals).

In general, the syntax is as following: `condition` ? `true_val` : `false_val`

Read following snippet of code and try to understand what it means:

```
resource "aws_db_instance" "read_replica" {
  count               = var.create_read_replica == true ? 1 : 0
  replicate_source_db = aws_db_instance.this.id
}
```

- `true`  # condition `if true`
- ?  # means, set to `1`
- : # means, otherwise, set to `0`

**Terraform Modules and best practices to structure your .tf codes**

By this time, you might have realized how difficult is to navigate through all the Terraform blocks if they are all written in a 
single long `.tf` file. As a **DevOps engineer**, you must produce reusable and comprehensive `IaC` code structure, and one of the tool that Terraform provides out of the box is **Modules**.

**Modules** serve as _containers_ that allow to logically group Terraform codes for similar resources in the same domain 
(e.g. Compute, Networking, AMI, etc.). One root module can call other child modules and insert their configurations when applying
Terraform config. This concept makes your code structure neater, and it allows different team members to work on different parts of
configuration at the same time. You can also create and publish your modules to Terraform Registry for others to use and use someone’s modules in your projects.

Module is just a collection of `.tf` and/or `.tf.json` files in a directory.

You can refer to existing child modules from your root module by specifying them as a source, like this:

```
module "network" {
  source = "./modules/network"
}
```
![image](https://github.com/user-attachments/assets/8693b617-005b-4339-ad60-e15a38feeb1d)

Note that the path to `network` module is set as relative to your working directory. 

Or you can also directly access resource outputs from the modules, like this:

```
resource "aws_elb" "example" {
  # ...
  instances = module.servers.instance_ids
}
```
In the example above, you will have to have module ‘servers’ to have output file to expose variables for this resource.

## REFACTOR YOUR PROJECT USING MODULES

Let us review the [repository](https://github.com/darey-devops/PBL-project-17) from project 17, you will notice that we had a single 
list of long file for creating all of our resources, but that is not the best way to go about it because it makes our code base very 
hard to read and understand therefore making future changes can be quite stressful.

QUICK TASK FOR YOU: Break down your Terraform codes to have all resources in their respective modules. Combine resources of a similar
type into directories within a ‘modules’ directory, for example, like this:

```
- modules
  - ALB: For Apllication Load balancer and similar resources
  - EFS: For Elastic file system resources
  - RDS: For Databases resources
  - Autoscaling: For Autosacling and launch template resources
  - compute: For EC2 and rlated resources
  - VPC: For VPC and netowrking resources such as subnets, roles, e.t.c.
  - security: for creating security group resources
```
![image](https://github.com/user-attachments/assets/50e6835a-5b39-49bd-b3dd-3e1f985a02a5)

Each module shall contain following files:

```
- main.tf (or %resource_name%.tf) file(s) with resources blocks
- outputs.tf (optional, if you need to refer outputs from any of these resources in your root module)
- variables.tf (as we learned before - it is a good practice not to hard code the values and use variables)
```
![image](https://github.com/user-attachments/assets/949bb1dc-c3be-415b-8c38-722d9831973d)

It is also recommended to configure providers and backends sections in separate files but should be placed in the root module.

![image](https://github.com/user-attachments/assets/628da44d-e385-47d7-b612-7f93513d8b18) ![image](https://github.com/user-attachments/assets/dfdaa15b-8d6d-4445-aa8a-af6ff1612631)




After you have given it a try, you can check out this [repository](https://github.com/darey-devops/PBL-project-18) 
for guidiance and erors fixing.

IMPORTANT: In the configuration sample from the repository, you can observe two examples of referencing the module:

a. Import module  as a source and have access to its variables via var keyword: in main.tf

```
module "VPC" {
  source = "./modules/VPC"
  region = var.region
  ...
```
![image](https://github.com/user-attachments/assets/cc7514a4-641b-48fd-af99-1632ee9948ca)


b. Refer to a module’s output by specifying the full path to the output variable by using module.%module_name%.%output_name% 
construction:


```
subnets-compute = module.network.public_subnets-1
```
![image](https://github.com/user-attachments/assets/c9ba715d-5684-4218-8ecd-ad32bad74b74)


# COMPLETE THE TERRAFORM CONFIGURATION

Complete the rest of the codes yourself, the resulting configuration structure in your working directory may look like this:

```
└── PBL
    ├── modules
    |   ├── ALB
    |     ├── ... (module .tf files, e.g., main.tf, outputs.tf, variables.tf)     
    |   ├── EFS
    |     ├── ... (module .tf files) 
    |   ├── RDS
    |     ├── ... (module .tf files) 
    |   ├── autoscaling
    |     ├── ... (module .tf files) 
    |   ├── compute
    |     ├── ... (module .tf files) 
    |   ├── network
    |     ├── ... (module .tf files)
    |   ├── security
    |     ├── ... (module .tf files)
    ├── main.tf
    ├── backends.tf
    ├── providers.tf
    ├── data.tf
    ├── outputs.tf
    ├── terraform.tfvars
    └── variables.tf
```
![image](https://github.com/user-attachments/assets/66ef101d-b40a-4cb1-8568-2eda76453f4e)

Now, the code is much more well-structured and can be easily read, edited and reused by your DevOps team members.

BLOCKERS: Your website would not be available because the userdata scripts we added to the launch template does not contain the 
latest endpoints for EFS, ALB and RDS and also our AMI is not properly configured, so how do we fix this?

**In project 19**, WE would see how to use **packer to create AMIs**, Terrafrom to create the infrastructure and Ansible to configure the
infrasrtucture.

We will also see how to use terraform cloud for our backends.

**Pro-tips:**

1. You can validate your codes before running terraform plan with terraform validate command. It will check if your code is 
syntactically valid and internally consistent.
```
terraform validate
```
![image](https://github.com/user-attachments/assets/242b73fb-5f43-42f7-b60d-54553e216805)


```
terraform plan
```
![image](https://github.com/user-attachments/assets/7ca72493-3e96-46ec-b03a-d22b089addd8)


2. In order to make your configuration files more readable and follow canonical format and style – use terraform fmt command. It will 
apply Terraform language style conventions and format your .tf files in accordance to them.

 Navigate to Your Project Directory

![image](https://github.com/user-attachments/assets/0ce0c5fe-6978-4201-a32b-71a688a8224c)

Simply run the terraform fmt command to format all Terraform files in your project directory 
```
terraform fmt
```

```
terraform fmt -check

```
![image](https://github.com/user-attachments/assets/5f3c396f-7bb9-485b-b85e-fa68d8f28532)


## The End of Project 18
we have done a great job developing and refactoring AWS Infrastructure as Code with Terraform!
