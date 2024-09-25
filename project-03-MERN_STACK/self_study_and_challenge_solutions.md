# Self Study Journey: Developing a MERN Stack Application in AWS
## 1. Self Study Journey

**Overview**

In my pursuit of mastering full-stack development, I chose to build a MERN (MongoDB, Express, React, Node.js) stack application deployed on AWS. This project was an opportunity to combine frontend and backend technologies with cloud deployment. By building this app, I aimed to enhance my proficiency in both MERN stack development and cloud services like AWS.

## Steps I Followed:

**Learning the MERN Stack:**

I started by learning the individual technologies in the MERN stack. I took several courses and worked on small projects using:

**MongoDB:** For database management, understanding CRUD operations, and data modeling.

**Express:** To create a robust backend with RESTful API design.

**React:**  For building dynamic user interfaces and managing application state.

**Node.js:**  As the server-side runtime environment for executing JavaScript code.


## Backend Development (Node.js and Express)

I learnt to build a REST API for the **to-do** app using Node.js and Express.I set up Express to create routes (GET, POST, PUT, DELETE) for managing to-do items.Explore mongoose for MongoDB integration to define a schema for storing tasks.

Example endpoints:

```
GET /api/todos  # Fetch all todos
POST /api/todos # Add new todo
DELETE /api/todos/:id # Delete a todo by id
```


## Frontend Development (React)
- I have  created a React application for interacting with the API.
- Develop components ( Input.js ListTodo.js Todo.js) to create, view and delete tasks.
- Use axios for making API requests to the backend.

##  MongoDB (Database)

- I have learnt how to  set up **MongoDB Atlas** for storing to-do items.

- I have learnt how to configure MongoDB Atlas and connect it to the Node.js app using mongoose.
- Secure MongoDB Atlas access by whitelisting IP addresses and using strong credentials.

## EC2 Instance Setup

- I learnt how to set up an **EC2** instance to host the Node.js backend.
- Launch an EC2 instance using Amazon Linux 2 or Ubuntu.
- SSH into the instance and configure the environment.
- Install Node.js, npm, and other necessary packages.
- Deploy the backend by uploading the codebase using SCP or Git, 

## Environment Variables

Store sensitive credentials (MongoDB URI) in environment variables using **.env** files and **dotenv** in Node.js.
## Continuous Learning

This journey reinforced the importance of continuous learning and adapting to new technologies. AWS is a constantly evolving platform, and staying up-to-date with new services and best practices is key to success.
Lesson: Embrace continuous learning to keep pace with changes in cloud technologies and development frameworks.

## Challenges Faced

- **Creating React App**

**Issue:** creating a React app was taking a long time

**Cause:**  The reason why creating a React app takes too long on an AWS t2.micro instance is likely due to the limited computing resources available on that instance type. The t2.micro instance has lower CPU power and less memory (1 vCPU and 1 GB of RAM), which can cause processes like installing dependencies and building the app to take significantly longer.

**Understanding AWS services:**

 Initially, it was challenging to navigate through the wide range of services AWS offers, but hands-on practice helped in understanding their roles.
Securing the MongoDB database: Managing access control and securing database credentials was critical and required a deeper understanding of networking.


**Solutions Implemented**


- **Upgrade Instance Type**

Upgrade an instance to a t2.small instance, which has more CPU power and double the memory (1 vCPU and 2 GB of RAM), the process speeds up because the system can handle the resource-intensive tasks more efficiently, leading to faster performance during app creation and builds.

