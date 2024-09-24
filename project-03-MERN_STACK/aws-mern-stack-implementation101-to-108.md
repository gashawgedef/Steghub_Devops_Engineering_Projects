
# MERN Stack Development

**MERN** stack development is a full-stack web development approach using **MongoDB**, **Express.js**, **React**, and **Node.js**, all based on JavaScript. MongoDB serves as the NoSQL database, while Express.js and Node.js handle the server-side logic, with React powering the dynamic user interface. 

This stack allows developers to build modern, scalable applications with a unified language, streamlining development across both frontend and backend. It is highly suitable for building single-page applications, offering efficiency through reusable React components, and enabling high performance with the asynchronous nature of Node.js.

## MERN Web Stack Components:

- **MongoDB**: A document-based, No-SQL database used to store application data in the form of documents.
- **Express.js**: A server-side web application framework for Node.js.
- **React.js**: A frontend framework developed by Facebook, based on JavaScript, used to build User Interface (UI) components.
- **Node.js**: A JavaScript runtime environment, used to run JavaScript on a machine rather than in a browser.

## Key Benefits:
- Single language (JavaScript) across the stack.
- Highly scalable and suitable for modern web applications.
- Efficient, with reusable React components and an asynchronous, event-driven backend.

## Getting Started a MERN STACK Project

**1. Launch instance in AWS Console**
The first step in implementing this project is to launch an instance in the AWS Console.

create an instance in the default region us-east-1 and enter instance name **MERN STACK WEB SERVER**

![image](assets//1_launch_instance.jpg)
- Next we select ubuntu operating system for an instance
  
![image](assets/2_select_os.jpg)

- Create or use existing  private key pair to log into the instance created
  
![image](assets/3_select_create_key_pair.jpg)

- Choose the **volume size** and **type** for the instance created

![image](assets/4_configure_storage.jpg)

- Configuring the security group in AWS EC2
  
A security group in AWS acts as a virtual firewall for your EC2 instances. It controls both inbound and outbound traffic to ensure only the permitted traffic reaches your instance. Each EC2 instance must be associated with at least one security group. The security group rules can be customized to define the type of traffic that is allowed to connect to the instance, including protocols, ports, and IP addresses.

![image](assets/5_configure_security_group.jpg)

- View the status of instance created

  ![image](assets/6_view_instance.jpg)

- View Instance Details

  ![image](assets/7_view_instance_details.jpg)

- Configuring Security Group with Specific Inbound Rules
  When setting up a security group for your EC2 instance, you can control which traffic reaches your instance through inbound rules.

For SSH (port 22), this rule allows secure shell access to your instance. By default, SSH is open to any IP address (0.0.0.0/0), which is useful for testing but insecure for production. It's recommended to restrict this access to trusted IPs to reduce exposure to unauthorized login attempts.

For HTTP (port 80), this rule allows web traffic from anywhere on the internet. This is essential if you're hosting a public-facing website or web service that users can access over HTTP.

For HTTPS (port 443), this rule enables secure web traffic. Like HTTP, HTTPS traffic is generally allowed from anywhere on the internet, ensuring encrypted access for your users. This is crucial for secure communication, especially for websites dealing with sensitive data.

By configuring these rules, your instance will allow SSH access for management and handle both HTTP and HTTPS traffic, making it accessible to the public while still maintaining necessary security measures.

  ![image](assets/8_create_inbound_rules.jpg)
- Connect to instance from ssh client

   ![image](assets/9_connect_instance.jpg)
**Give Permission for the Private SSH Key**
  
  This command ensures that this  private SSH key has the correct permissions before using it to connect to your instance.

  ```
  chmod 400 "gashaw_key.pem"
  ```
  ![image](assets/10_give_permision.jpg)
**Connecting to the Instance via SSH**

Once the private key file has the correct permissions, you can use SSH to connect to your EC2 instance using its public IP address or domain name.
```
ssh -i "gashaw_key.pem" ubuntu@ec2-54-226-142-99.compute-1.amazonaws.com
```
![image](assets/11_connect_to_ssh_client.jpg)
## Step 1 - Backend configuration
- Update ubuntu

```
sudo apt update
```

![image](assets/13_update_ubuntu.jpg)


Lets get the location of Node.js software from Ubuntu repositories.

```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

- **Install Node.js on the server**

```
sudo apt-get install -y nodejs
```
![image](assets/15_install_Node_js.jpg)

- Verify the node installation with the command below
```
node -v 
```

![image](assets/16_verify_node_instalation.jpg)

 - **Application Code Setup**

Create a new directory for your To-Do project:

```
mkdir Todo
```

![image](assets/17_create_todo_directory.jpg)

Now change your current directory to the newly created one:

```
 cd Todo
 ```

Next, you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.

![image](assets/18_npm_int.jpg)

## Install ExpressJS

Express.js is a lightweight and flexible web application framework for Node.js, designed to simplify building server-side applications and APIs. It offers features for handling routing, middleware, and HTTP requests, making it ideal for creating scalable and efficient web applications.

- To use express, install it using npm:

```
npm install express
```
![image](assets/19_install_express_js.jpg)

- Create a file **index.js** with the command below

```
 touch index.js
```
![image](assets/20_create_index_js_file.jpg)

- Install the dotenv module

```
npm install dotenv
```
![image](assets/21_install_dotenv.jpg)

- Open the index.js file with the command below

```
vim index.js
```
Type the code below into it and save.

```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```
![image](assets/22_edit_code_vim.jpg)


Specify port 5000 in the AWS instance's inbound rules.

Now it is time to start our server to see if it works. Open your terminal in the same directory as your index.js file and type:

```
node index.js
```
![image](assets/23_runn_nodejs_server.jpg)

Open up your browser and try to access your server's Public IP or Public DNS name followed by port 5000:

```
http://18.232.79.101:5000
```
![image](assets/24_open_in_public_ip_address.jpg)

## Routes
There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display list of all tasks
3. Delete a completed task
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: **POST**, **GET**, **DELETE**.

- Create **routes** folder

```
mkdir routes
```
![image](assets/25_create_routes_folder.jpg)

- Change directory to routes folder.

![image](assets/26_change_to_routes_directory.jpg)

- create a file api.js with the command below

```
touch api.js
```

![image](assets/27_create_api_js.jpg)

- Open the file with the command below

```
vim api.js
```
Copy below code in the file. 

```
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
```

![image](assets/28_edit_api_js.jpg)


## Models

A **model** is central to JavaScript-based applications, enabling interactivity and defining the database schema for MongoDB documents. The schema serves as a blueprint for the database structure, outlining stored fields and additional virtual properties that may not be required. To create a schema and model, you should install Mongoose, a Node.js package that simplifies interactions with MongoDB.

- Change directory back Todo folder 

```
cd ..
```
![image](assets/29_change_directory_to_Todo.jpg)

- install Mongoose

```
npm install mongoose
```

![image](assets/30_install_mongo_db.jpg)

Create a new folder with **models** with the following command

```
mkdir models
```
![image](assets/31_create_models_folder_and_navigate.jpg)

Change directory into the newly created **models** folder with cd models.

```
cd models
```

Inside the **models** folder, create a file and name it **todo.js**

```
touch todo.js
```
![image](assets/32_create_todo_js_file_inside_models.jpg)

- Open the file created with **vim todo.js**

```
vim todo.js
```
-  then paste the code below in the file:

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```
![Image](assets/33_vim_edit_todo_js.jpg)

Now we need to update our routes from the file **api.js** in **routes** directory to make use of the new model.

In Routes directory, open api.js with **vim api.js**, delete the code inside with **:%d** command and paste there code below into it then save and exit

```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;
```

![image](assets/34_edit_api_js.jpg)


## MongoDB Database

MongoDB is a NoSQL database that stores data in flexible, JSON-like documents, allowing for dynamic schemas and easy management of unstructured data. Known for its scalability and performance, it handles large volumes of data across distributed systems. MongoDB supports powerful querying, indexing, and real-time analytics, making it popular for building scalable web and mobile applications.

![image](assets/36_config_mongo_db.jpg)

Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)

![image](assets/35_add_ip_from_anywhere.jpg)

- Create a MongoDB database and collection inside mLab

![image](assets/39_create_database.jpg)

- Database after created looks like

![image](assets/40_after_created_database.jpg)

In the **index.js** file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.

Create a file in your Todo directory and name it **.env**.

```
touch .env
```
![image](assets/41_create_env_file.jpg)


- Edit Environment varibale **.env**

```
vim .env
```

- Paste this file
```
DB='mongodb+srv://gashawgedef:*****************@cluster0.mnsga.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0'
```
Here is how to get your connection string
![image](assets/37_connect.jpg)

![image](assets/38_connect.jpg)

![image](assets/42_edit_env_variable.jpg)

Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.

Simply delete existing content in the file, and update it with the entire code below.

```
vim index.js
```
- Add the following code after deleting the first content

```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```

![image](assets/43_edit_index_js_file.jpg)

- Start your server using the command:

```
node index.js
```

![image](assets/4)