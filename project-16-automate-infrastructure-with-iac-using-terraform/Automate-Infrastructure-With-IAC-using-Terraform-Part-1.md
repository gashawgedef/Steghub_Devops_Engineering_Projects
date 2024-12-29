# AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM PART 1

After you have built AWS infrastructure for 2 websites manually, it is time to automate the process using Terraform.

Let us start building the same set up with the power of Infrastructure as Code (IaC)

![image](assets/352381474-76245f5b-f77a-4686-8830-c52b178aa937.png)


## Prerequisites before you begin writing Terraform code
- You must have completed Terraform course from the Learning dashboard
- Create an IAM user, name it `terraform` (ensure that the user has only programatic access to your AWS account) and grant this user 
AdministratorAccess permissions.
![image](assets/pr16-01-create-teraform-user.JPG)

![image](assets/pr16-02-give-administrator-access.JPG)

- Copy the secret access key and access key ID. Save them in a notepad temporarily.
![image](assets/pr16-03-create-access-keys.JPG)

- Configure programmatic access from your workstation to connect to AWS using the access keys copied above and a 
[Python SDK (boto3)](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html).

 You must have Python 3.6 or higher on your workstation.
```
python --version
```
![image](assets/pr16-07-see-python-version.JPG)

If you are on Windows, use gitbash, if you are on a Mac, you can simply open a terminal. Read here to configure the Python SDK properly.
```
aws --version
```
![image](assets/pr16-08-aws-version.JPG)

For easier authentication configuration – use AWS CLI with aws configure command.

Create an S3 bucket to store Terraform state file. You can name it something like `yourname`-dev-terraform-bucket 
(Note: S3 bucket names must be unique unique within a region partition, you can read about S3 bucken naming  ). 
We will use this bucket from Project-17 onwards.
for me `gashaw-dev-terraform-bucket`
![image](assets/pr16-04-create_buckets.JPG)

![image](assets/pr16-05-createbucket-02.JPG)


 **Verfiy this in  AWS CLI**
 ```
aws s3 ls
```
![image](assets/pr16-09-see-bucket-list.JPG)



Install Boto3 (Boto3 is a AWS SDK for Python) in your local machine 
```
pip install boto3
```
![images](assets/pr16-10-install-python-sdk.JPG)
When you have configured authentication and installed boto3, make sure you can programmatically access your AWS account by running 
following commands in >python:
  
```
import boto3
s3 = boto3.resource('s3')
for bucket in s3.buckets.all():
    print(bucket.name)
```
Save the above code to a file (e.g., list_buckets.py) or run it directly in a Python interpreter
```
 python gashaw.py
```
  
You shall see your previously created S3 bucket name – gashaw-dev-terraform-bucket
![image](assets/pr16-06-retrieve-buckets.JPG)

## The secrets of writing quality Terraform code
The secret recipe of a successful Terraform projects consists of:

- Your understanding of your goal (desired AWS infrastructure end state)
- Your knowledge of the IaC technology used (in this case – Terraform)
- Your ability to effectively use up to date Terraform documentation [here](https://developer.hashicorp.com/terraform/language)

As you go along completing this project, you will get familiar with 
[Terraform-specific terminology](https://developer.hashicorp.com/terraform/docs/glossary), such as:


- Attribute
- Resource
- Interpolations
- Argument
- Providers
- Provisioners
- Input Variables
- Output Variables
- Module
- Data Source
- Local Values
- Backend

  
Make sure you understand them and know when to use each of them.

Another concept you must know is data type. This is a general programing concept, it refers to how data represented in a programming 
language and defines how a compiler or interpreter can use the data. Common data types are:

- Integer
- Float
- String
- Boolean, etc.


## Best practices

- Ensure that every resource is tagged using multiple key-value pairs. You will see this in action as we go along.
- Try to write reusable code, avoid hard coding values wherever possible. (For learning purpose, we will start by hard coding,
but gradually refactor our work to follow best practices).


# VPC | SUBNETS | SECURITY GROUPS

Let us create a directory structure
Open your Visual Studio Code and:

- Create a folder called PBL
- Create a file in the folder, name it main.tf

Your setup should look like this.

![image](assets/PR16-11-CREATE-PBL.JPG)

## Provider and VPC resource section
Set up Terraform CLI as per this [instruction](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli).

- Add AWS as a provider, and a resource to create a VPC in the main.tf file.
- Provider block informs Terraform that we intend to build infrastructure within AWS.
- Resource block will create a VPC.



```
provider "aws" {
  region = "us-east-1"
}

# Create VPC
resource "aws_vpc" "main" {
  cidr_block                     = "172.16.0.0/16"
  enable_dns_support             = "true"
  enable_dns_hostnames           = "true"
  enable_classiclink             = "false"
  enable_classiclink_dns_support = "false"
}

```


**Note:** You can change the configuration above to create your VPC in other region that is closer to you. The same applies to all 
configuration snippets that will follow.

- The next thing we need to do, is to download necessary plugins for Terraform to work. These plugins are used by providers and
provisioners. At this stage, we only have provider in our main.tf file. So, Terraform will just download plugin for AWS provider.

- Lets accomplish this with terraform init command as seen in the below demonstration.
```
> cd PBL

> terraform init
```
![image](assets/pr16-15-init.jpg)


**Observations:**

- Notice that a new directory has been created: .terraform\.... This is where Terraform keeps plugins. Generally, it is safe to 
delete this folder. It just means that you must execute terraform init again, to download them.

Moving on, let us create the only resource we just defined. aws_vpc. But before we do that, we should check to see what terraform 
intends to create before we tell it to go ahead and create it.
Let's verify what terraform intends to create

```
terraform plan
```
![image](assets/pr16-16-plan-plan.jpg)

- Then, if you are happy with changes planned, execute

```
 terraform apply
```
![images](assets/pr16-17-terraform-apply.jpg)

**Observations:**

1. A new file is created terraform.tfstate This is how Terraform keeps itself up to date with the exact state of the infrastructure. 
It reads this file to know what already exists, what should be added, or destroyed based on the entire terraform code that is being
developed.

2. If you also observed closely, you would realise that another file gets created during planning and apply. But this file gets 
deleted immediately. terraform.tfstate.lock.info This is what Terraform uses to track, who is running its code against the 
infrastructure at any point in time. This is very important for teams working on the same Terraform repository at the same time. 
The lock prevents a user from executing Terraform configuration against the same infrastructure when another user is doing the 
same – it allows to avoid duplicates and conflicts.


Its content is usually like this. (We will discuss more about this later)

```
{
        "ID":"e5e5ad0e-9cc5-7af1-3547-77bb3ee0958b",
        "Operation":"OperationTypePlan","Info":"",
        "Who":"ann@ann","Version":"0.13.4",
        "Created":"2020-10-28T19:19:28.261312Z",
        "Path":"terraform.tfstate"
    }
```

It is a json format file that stores information about a user: user’s ID, what operation he/she is doing, timestamp, and location 
of the state file.

## Subnets resource section
According to our architectural design, we require 6 subnets:

- 2 public
- 2 private for webservers
- 2 private for data layer

Let us create the first 2 public subnets.

Add below configuration to the main.tf file:


```
# Create public subnets1
    resource "aws_subnet" "public1" {
    vpc_id                     = aws_vpc.main.id
    cidr_block                 = "172.16.0.0/24"
    map_public_ip_on_launch    = true
    availability_zone          = "us-east-1a"

}

# Create public subnet2
    resource "aws_subnet" "public2" {
    vpc_id                     = aws_vpc.main.id
    cidr_block                 = "172.16.1.0/24"
    map_public_ip_on_launch    = true
    availability_zone          = "us-east-1b"
}
```


- We are creating 2 subnets, therefore declaring 2 resource blocks – one for each of the subnets.
- We are using the vpc_id argument to interpolate the value of the VPC id by setting it to aws_vpc.main.id. This way, Terraform knows
inside which VPC to create the subnet.
Run terraform plan to check the intend infrusture and terraform apply to create the infrusture.

```
terraform plan
```
![images](assets/pr16-18_create-subnets.jpg)
```
terraform apply
```
![image](assets/pr16-20-apply-subnets.jpg)

![image](assets/pr16-22-vpc.jpg)

![image](assets/pr16-21-subnets%20list.jpg)


**Observations:**

- Hard coded values: Remember our best practice hint from the beginning? Both the availability_zone and cidr_block arguments are hard 
coded. We should always endeavour to make our work dynamic.
- Multiple Resource Blocks: Notice that we have declared multiple resource blocks for each subnet in the code. This is bad 
coding practice. We need to create a single resource block that can dynamically create resources without specifying multiple blocks.
Imagine if we wanted to create 10 subnets, our code would look very clumsy. So, we need to optimize this by introducing a count
argument.

First, destroy the current infrastructure. Since we are still in development, this is totally fine. Otherwise, DO NOT DESTROY an infrastructure that has been deployed to production.
```
terraform destroy
```
![image](assets/pr16-23-destroy.jpg)

Now let us improve our code by refactoring it.

# FIXING THE PROBLEMS BY CODE REFACTORING

- Fixing Hard Coded Values: We will introduce variables, and remove hard coding.
- Starting with the provider block, declare a variable named region, give it a default value, and update the provider section by referring to the declared variable.
   
```
variable "region" {
        default = "us-east-1"
    }

    provider "aws" {
        region = var.region
    }
```

- Do the same to cidr value in the vpc block, and all the other arguments.

```
variable "region" {
        default = "us-east-1"
    }

    variable "vpc_cidr" {
        default = "172.16.0.0/16"
    }

    variable "enable_dns_support" {
        default = "true"
    }

    variable "enable_dns_hostnames" {
        default ="true" 
    }

    variable "enable_classiclink" {
        default = "false"
    }

    variable "enable_classiclink_dns_support" {
        default = "false"
    }

    provider "aws" {
    region = var.region
    }

    # Create VPC
    resource "aws_vpc" "main" {
    cidr_block                     = var.vpc_cidr
    enable_dns_support             = var.enable_dns_support 
    enable_dns_hostnames           = var.enable_dns_support
    enable_classiclink             = var.enable_classiclink
    enable_classiclink_dns_support = var.enable_classiclink

    }
```


- **Fixing multiple resource blocks**: This is where things become a little tricky. It’s not complex, we are just going to introduce
 some interesting concepts. Loops & Data sources

Terraform has a functionality that allows us to pull data which exposes information to us. For example, every region has Availability
Zones (AZ). Different regions have from 2 to 4 Availability Zones. With over 20 geographic regions and over 70 AZs served by AWS, 
it is impossible to keep up with the latest information by hard coding the names of AZs. Hence, we will explore the use of Terraform’s
Data Sources to fetch information outside of Terraform. In this case, from AWS

Let us fetch Availability zones from AWS, and replace the hard coded value in the subnet’s availability_zone section.


```
        # Get list of availability zones
        data "aws_availability_zones" "available" {
        state = "available"
        }
```


To make use of this new data resource, we will need to introduce a `count` argument in the subnet block: Something like this.


```
 # Create public subnet1
    resource "aws_subnet" "public" { 
        count                   = 2
        vpc_id                  = aws_vpc.main.id
        cidr_block              = "172.16.1.0/24"
        map_public_ip_on_launch = true
        availability_zone       = data.aws_availability_zones.available.names[count.index]

    }
```


Let us quickly understand what is going on here.

- The count tells us that we need 2 subnets. Therefore, Terraform will invoke a loop to create 2 subnets.
- The data resource will return a list object that contains a list of AZs. Internally, Terraform will receive the data like this

```
["us-east-1a", "us-east-1b"]
```

Each of them is an index, the first one is index 0, while the other is index 1. If the data returned had more than 2 records, then 
the index numbers would continue to increment.

Therefore, each time Terraform goes into a loop to create a subnet, it must be created in the retrieved AZ from the list. Each loop 
will need the index number to determine what AZ the subnet will be created. That is why we have 
data.aws_availability_zones.available.names[count.index] as the value for availability_zone. When the first loop runs, the first
index will be 0, therefore the AZ will be eu-central-1a. The pattern will repeat for the second loop.

But we still have a problem. If we run Terraform with this configuration, it may succeed for the first time, but by the time it goes
into the second loop, it will fail because we still have cidr_block hard coded. The same cidr_block cannot be created twice within
the same VPC. So, we have a little more work to do.

## Let’s make cidr_block _dynamic_.

We will introduce a function cidrsubnet() to make this happen. It accepts 3 parameters. Let us use it first by updating the
configuration, then we will explore its internals.

```
# Create public subnet1
    resource "aws_subnet" "public" { 
        count                   = 2
        vpc_id                  = aws_vpc.main.id
        cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
        map_public_ip_on_launch = true
        availability_zone       = data.aws_availability_zones.available.names[count.index]

    }
```

A closer look at cidrsubnet – this function works like an algorithm to dynamically create a subnet CIDR per AZ. Regardless of the
number of subnets created, it takes care of the cidr value per subnet.

Its parameters are cidrsubnet(prefix, newbits, netnum)

- The prefix parameter must be given in CIDR notation, same as for VPC.
- The newbits parameter is the number of additional bits with which to extend the prefix. For example, if given a prefix ending with 
/16 and a newbits value of 4, the resulting subnet address will have length /20
- The netnum parameter is a whole number that can be represented as a binary integer with no more than newbits binary digits, which
will be used to populate the additional bits added to the prefix


You can experiment how this works by entering the terraform console and keep changing the figures to see the output.


- On the terminal, run terraform console
- type cidrsubnet("172.16.0.0/16", 4, 0)
- Hit enter
- See the output
- Keep change the numbers and see what happens.
- To get out of the console, type exit


## The final problem to solve is removing hard coded count value.

If we cannot hard code a value we want, then we will need a way to dynamically provide the value based on some input. Since the data
resource returns all the AZs within a region, it makes sense to count the number of AZs returned and pass that number to the count
argument.

To do this, we can introuduce length() function, which basically determines the length of a given list, map, or string.

Since data.aws_availability_zones.available.names returns a list like ["us-east-1a", "us-east-1b", "us-east-1c"] we can pass
it into a lenght function and get number of the AZs.

length(["us-east-1a", "us-east-1b", "us-east-1c"])

Open up terraform console and try it



Now we can simply update the public subnet block like this


```
# Create public subnet1
    resource "aws_subnet" "public" { 
        count                   = length(data.aws_availability_zones.available.names)
        vpc_id                  = aws_vpc.main.id
        cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
        map_public_ip_on_launch = true
        availability_zone       = data.aws_availability_zones.available.names[count.index]

    }
```


**Observations:**

- What we have now, is sufficient to create the subnet resource required. But if you observe, it is not satisfying our business
requirement of just 2 subnets. The length function will return number 3 to the count argument, but what we actually need is 2.

Now, let us fix this.

- Declare a variable to store the desired number of public subnets, and set the default value

```
variable "preferred_number_of_public_subnets" {
  default = 2
}
```


- Next, update the count argument with a condition. Terraform needs to check first if there is a desired number of subnets. Otherwise,
 use the data returned by the lenght function. See how that is presented below.
 
 
```
# Create public subnets
resource "aws_subnet" "public" {
  count  = var.preferred_number_of_public_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_public_subnets   
  vpc_id = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
  map_public_ip_on_launch = true
  availability_zone       = data.aws_availability_zones.available.names[count.index]

}
```


Now lets break it down:

- The first part var.preferred_number_of_public_subnets == null checks if the value of the variable is set to null or has some value
 defined.
- The second part ? and length(data.aws_availability_zones.available.names) means, if the first part is true, then use this. In other
 words, if preferred number of public subnets is null (Or not known) then set the value to the data returned by lenght function.
- The third part : and var.preferred_number_of_public_subnets means, if the first condition is false, i.e preferred number of public
 subnets is not null then set the value to whatever is definied in var.preferred_number_of_public_subnets
 
 
Now the entire configuration should now look like this


```
# Get list of availability zones
data "aws_availability_zones" "available" {
  state = "available"
}

variable "region" {
  default = "us-east-1"
}

variable "vpc_cidr" {
  default = "172.16.0.0/16"
}

variable "enable_dns_support" {
  default = "true"
}

variable "enable_dns_hostnames" {
  default = "true"
}

variable "preferred_number_of_public_subnets" {
  default = 2
}

provider "aws" {
  region = var.region
}

# Create VPC
resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_support   = var.enable_dns_support
  enable_dns_hostnames = var.enable_dns_hostnames
}

# Create public subnets
resource "aws_subnet" "public" {
  count  = var.preferred_number_of_public_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_public_subnets
  vpc_id = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 4, count.index)
  map_public_ip_on_launch = true
  availability_zone       = data.aws_availability_zones.available.names[count.index]
}

```

![image](assets/pr16-25-creates.jpg)

![image](assets/pr16-24-create.jpg)


Note: You should try changing the value of preferred_number_of_public_subnets variable to null and notice how many subnets get 
created.



> First, destroy the current infrastructure. Since we are still in development, this is totally fine. Otherwise, DO NOT DESTROY an 
infrastructure that has been deployed to production.

To destroy whatever has been created run terraform destroy command, and type yes after evaluating the plan.
```
terraform destroy
```
# Introducing `variables.tf` & `terraform.tfvars`

Instead of havng a long lisf of variables in main.tf file, we can actually make our code a lot more readable and better structured by 
moving out some parts of the configuration content to other files.

- We will put all variable declarations in a separate file
- And provide non default values to each of them

1. Create a new file and name it variables.tf
2. Copy all the variable declarations into the new file.
3. Create another file, name it terraform.tfvars
4. Set values for each of the variables.


**`Maint.tf`**

```
# Get list of availability zones
data "aws_availability_zones" "available" {
state = "available"
}

provider "aws" {
  region = var.region
}

# Create VPC
resource "aws_vpc" "main" {
  cidr_block                     = var.vpc_cidr
  enable_dns_support             = var.enable_dns_support 
  enable_dns_hostnames           = var.enable_dns_support
  enable_classiclink             = var.enable_classiclink
  enable_classiclink_dns_support = var.enable_classiclink

}

# Create public subnets
resource "aws_subnet" "public" {
  count  = var.preferred_number_of_public_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_public_subnets   
  vpc_id = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
  map_public_ip_on_launch = true
  availability_zone       = data.aws_availability_zones.available.names[count.index]
}
```
![image](assets/pr16-26-main-tf.jpg)



**`variables.tf`**

```
variable "region" {
      default = "us-east-1"
}

variable "vpc_cidr" {
    default = "172.16.0.0/16"
}

variable "enable_dns_support" {
    default = "true"
}

variable "enable_dns_hostnames" {
    default ="true" 
}

variable "enable_classiclink" {
    default = "false"
}

variable "enable_classiclink_dns_support" {
    default = "false"
}

  variable "preferred_number_of_public_subnets" {
      default = null
}
```
![image](assets/pr16-27-values.jpg)


**`terraform.tfvars`**

```
region = "us-east-1"

vpc_cidr = "172.16.0.0/16" 

enable_dns_support = "true" 

enable_dns_hostnames = "true"  

enable_classiclink = "false" 

enable_classiclink_dns_support = "false" 

preferred_number_of_public_subnets = 2
```
![image](assets/pr16-28-variables-tft.jpg)


You should also have this file structure in the PBL folder.

```
└── PBL
    ├── main.tf
    ├── terraform.tfstate
    ├── terraform.tfstate.backup
    ├── terraform.tfvars
    └── variables.tf
```
![image](assets/pr16-29-file-structure.jpg)


Run terraform plan and ensure everything works
`Plan`
![image](assets/pr16-30-plan.jpg)

`apply`

![image](assets/pr16-31-create-files.jpg)

`Verify VPC in AWS management Console`
![image](assets/pr16-32-see-vpc.jpg)

![image](assets/pr16-33-see-subnets.jpg)

```
# Delete the infastructure
terraform destroy
```
![image](assets/pr16-34-delete.jpg)

### The End of Project 16 
In this project we have learned how to create and delete AWS Network Infrastructure programmatically with Terraform!

In the next project we will expand the infrastructure further with more Terraform automation and learn more AWeSome stuff.

Move on to the next project!

