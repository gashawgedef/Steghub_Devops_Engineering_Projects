# Self Study Journey: Developing a MERN Stack Application in AWS
## 1. Self Study Journey

**Overview**

In my pursuit of mastering full-stack development, I chose to build a MERN (MongoDB, Express, React, Node.js) stack application deployed on AWS. This project was an opportunity to combine frontend and backend technologies with cloud deployment. By building this app, I aimed to enhance my proficiency in both MERN stack development and cloud services like AWS.

## Steps I Followed:

**Learning the MERN Stack:**

I started by learning the individual technologies in the MERN stack. I took several courses and worked on small projects using MongoDB, Express, React, and Node.js to gain practical experience.

**Building the Application:**

I designed and built a full-stack application. The backend was developed using Node.js with Express, and the frontend was built using React. MongoDB was used for database management.

**Deploying on AWS:**

I familiarized myself with AWS services such as EC2 (Elastic Compute Cloud), S3 (Simple Storage Service), and RDS (Relational Database Service) to host the application.

I used EC2 to host my server-side code and deployed my frontend via S3. I also explored AWS Elastic Beanstalk for deployment automation.


**2. Challenges Faced**

- **Creating React App**

**Issue:** creating a React app was taking a long time

**Cause:**  The reason why creating a React app takes too long on an AWS t2.micro instance is likely due to the limited computing resources available on that instance type. The t2.micro instance has lower CPU power and less memory (1 vCPU and 1 GB of RAM), which can cause processes like installing dependencies and building the app to take significantly longer.


**Solutions Implemented**


- **Upgrade Instance Type**

Upgrade an instance to a t2.small instance, which has more CPU power and double the memory (1 vCPU and 2 GB of RAM), the process speeds up because the system can handle the resource-intensive tasks more efficiently, leading to faster performance during app creation and builds.


**Lessons Learned**

- Cloud Service Mastery

AWS provides a wide range of services, and while this flexibility is powerful, it can also be overwhelming. I learned how to selectively use services like EC2, S3, and CloudFront efficiently.
Lesson: Proper planning and understanding AWS architecture before deployment is critical to ensure both security and scalability.

- MERN Stack Deployment

Full-stack applications require careful orchestration between frontend and backend services, especially during deployment in the cloud. Understanding how routing works in SPAs when deployed to a static environment was crucial.
Lesson: Cloud deployment introduces additional complexities such as handling static assets, API endpoints, and database connections.

- Continuous Learning

The journey taught me the importance of continuously learning and adapting to new technologies. AWS itself is a constantly evolving platform, and staying updated is key.
Lesson: Embrace continuous learning to keep up with changes in cloud technologies and development frameworks.