
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

 **Application Code Setup**

- Create a new directory for your To-Do project:

```
mkdir Todo
```

![image](assets/17_create_todo_directory.jpg)


- Now change your current directory to the newly created one:

```
 cd Todo
 ```

Next, you will use the command **npm init** to initialise your project, so that a new file named **package.json** will be created.

This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.


![image](assets/18_npm_int.jpg)


## Install ExpressJS

**Express.js**  is a lightweight and flexible web application framework for **Node.js**, designed to simplify building server-side applications and APIs. It offers features for handling routing, middleware, and HTTP requests, making it ideal for creating scalable and efficient web applications.

- To use **express**, install it using npm:


```
npm install express
```

![image](assets/19_install_express_js.jpg)


- Create a file **index.js** with the command below

```
 touch index.js
```
![image](assets/create_index_file.jpg)

- Install the dotenv module

```
npm install dotenv
```

![image](assets/21_install_dotenv.jpg)


- Open the index.js file with the command below

```
vim index.js
```

![image](assets/open_index_js.jpg)


- Type the code below into it and save.


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


- Specify port 5000 in the AWS instance's inbound rules.

![image](assets/inbound%20Rules%205000.jpg)

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


- create a file **api.js** with the command below


```
touch api.js
```

![image](assets/27_create_api_js.jpg)

- Open the file with the command below

```
vim api.js
```

![image](assets/open_api_js.jpg)


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

![image](assets/open_todo_js.jpg)


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

**MongoDB** is a NoSQL database that stores data in flexible, JSON-like documents, allowing for dynamic schemas and easy management of unstructured data. Known for its scalability and performance, it handles large volumes of data across distributed systems. MongoDB supports powerful querying, indexing, and real-time analytics, making it popular for building scalable web and mobile applications.

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

Now we need to update the **index.js** to reflect the use of .env so that Node.js can connect to the database.

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

![image](assets/44_database_connected_successfully.jpg)


## Testing Backend Code without Frontend using RESTful API

So far, we've built the backend for our To-Do application and set up the database, but the frontend UI using ReactJS is still pending. During development, we need to test the backend functionality through the RESTful API. To do this efficiently before the frontend is ready, we can use an API development client like Postman or Insomnia to test our API endpoints and ensure the backend works correctly.

Now open your Postman, create a POST request to the API http://<PublicIP-or-PublicDNS>:5000/api/todos. This request sends a new task to our To-Do list so the application could store it in the database.

Note: make sure your set header key Content-Type as application/json


![image](assets/45_retrive_before_insert.jpg)


- Make HTTP POST request

![image](assets/post_request.jpg)


- Create a GET request to your API


```
http://54.90.167.28:5000/api/todos
```

![image](assets/47_check_data_is_inserted.jpg)

- DELETE request to delete a task from out To-Do list.

![image](assets/48_delete_using_id.jpg)

- Check After being Deleted

![image](assets/49_check_after_delete.jpg)

## Step 2 - Frontend creation

Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.

In the same root directory as your backend code, which is the Todo directory, run:

```
 npx create-react-app client
```


![image](assets/51_create_client_react_app_copp.jpg)


- Install **concurrently**. It is used to run more than one command simultaneously from the same terminal window.

```
npm install concurrently --save-dev
```

![image](assets/52_install_concurently.jpg)

- Install **nodemon**. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.

```
npm install nodemon --save-dev
```

![image](assets/51_install_nodemon.jpg)


- In **Todo** folder open the **package.json** file. Change the highlighted part of the below screenshot and replace with the code below.

```
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```

![image](assets/53_vim_package_json.jpg)


- Then the code is replaced as follows

![image](assets/54_edit_package.json.jpg)

**Configure Proxy in package.json**

- Change directory to 'client'

```
cd client
```


```
 vim package.json
```


![image](assets/55_add_proxy.jpg)



Now, ensure you are inside the Todo directory, and simply do:


```
npm run dev

```


![image](assets/58_run_react_app.jpg)


## Creating your React Components

One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.
- From your Todo directory run:

```
cd client
```


- move to the src directory

```
cd src
```


![image](assets/60_goto_src_folder.jpg)


- Inside your src folder create another folder called components


```
mkdir components
```


![image](assets/61_create_components_folder.jpg)


- Move into the components directory with

```
cd components

```

- Inside **components** directory create three files Input.js, ListTodo.js and Todo.js.

```
touch Input.js ListTodo.js Todo.js
```

![image](assets/62_create_3_Files_in_files.jpg)


- Open **Input.js** file

```
vim Input.js
```

![image](assets/63_open_input_js.jpg)

- Copy and paste the following


```
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
```


![image](assets/65_edit_input_js.jpg)


To make use of **Axios**, which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

- Move to the **src** folder

```
cd ..
```

![image](assets/66_move_src_folder.jpg)

- Move to **clients** folder

```
cd ..
```

![image](assets/67_move_clients_folder.jpg)

- Install **Axios**


```
npm install axios
```


![image](assets/70_install_axios.jpg)


- Go to 'components' directory

```
cd src/components
```


![image](assets/71_move_to_components.jpg)


- After that open your ListTodo.js


```
vim ListTodo.js

```

![image](assets/72_open_list_todo.jpg)


- In the ListTodo.js copy and paste the following code

```
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
```

![image](assets/73_edit_list_todo.jpg)


- Then in your **Todo.js** file you write the following code


```
import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
```

![image](assets/75_edit_todo_js.jpg)


We need to make little adjustment to our react code. Delete the logo and adjust our App.js to look like this.

- Move to the **src** folder


```
cd ..
```

- Make sure that you are in the src folder and run


```
vim App.js
```

- Copy and paste the code below into it


```
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
```


![image](assets/77_edit_app_js.jpg)


After pasting, exit the editor.

- In the **src** directory open the App.css

```
vim App.css
```

![image](assets/78_open_app_css.jpg)


- Then paste the following code into App.css:


```
.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
```

![image](assets/79_edit_app_css.jpg)


- In the src directory open the index.css


```
vim index.css
```


![image](assets/80_open_index_css.jpg)


- Copy and paste the code below


```
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
```

![image](assets/81_edit_index_css.jpg)

- Go to the **Todo** directory

```
cd ../..
```
![image](assets/82_move_to_todo_folder.jpg)

- When you are in the **Todo** directory run:

```
npm run dev

```

![image](assets/npm%20run%20final.JPG)


## The End Of MERN STACK Project
A MERN stack application (MongoDB, Express, React, Node.js) deployed on AWS offers a full-stack solution where the backend is hosted using AWS EC2 for server-side operations, MongoDB Atlas for cloud-based database management, and the frontend served via AWS S3 for static hosting. The application typically involves RESTful APIs for data communication between the client and server,