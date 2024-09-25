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


- **Backend Integration**

Issue: Setting up the connection between the backend (Node.js/Express) and MongoDB hosted on AWS was challenging due to security settings.
Cause: AWS VPC (Virtual Private Cloud) configurations and firewall rules blocked access to the database.

- **Creating React App**

Issue: Deploying the React application using AWS S3 had issues with routing.
Cause: S3 static website hosting does not handle client-side routing for single-page applications (SPAs) well.

- **AWS Costs & Resource Management**

Issue: Managing AWS resources efficiently while keeping costs low was a new experience.
Cause: The variety of services and pricing options required careful planning and resource allocation.

**Solutions Implemented**

- **Securing the Database**

I configured AWS Security Groups to allow only my EC2 instance to access the MongoDB instance. I also enabled VPC Peering to improve security within the private network.
Implemented environment variables using AWS Systems Manager Parameter Store to securely manage database credentials.

- **Handling Frontend Routing**

I solved the routing issue by configuring AWS CloudFront as a CDN to serve my React app and setting up custom error pages to handle the client-side routing for undefined routes.

- **Cost Optimization**

I used AWS Free Tier services where possible and configured Auto Scaling for my EC2 instance to adjust resources based on traffic.
Set up CloudWatch monitoring and created alerts to track resource usage and avoid unnecessary costs.


**Lessons Learned**

- Cloud Service Mastery

AWS provides a wide range of services, and while this flexibility is powerful, it can also be overwhelming. I learned how to selectively use services like EC2, S3, and CloudFront efficiently.
Lesson: Proper planning and understanding AWS architecture before deployment is critical to ensure both security and scalability.

- MERN Stack Deployment

Full-stack applications require careful orchestration between frontend and backend services, especially during deployment in the cloud. Understanding how routing works in SPAs when deployed to a static environment was crucial.
Lesson: Cloud deployment introduces additional complexities such as handling static assets, API endpoints, and database connections.

- Security Considerations

Managing security in the cloud requires extra caution. Misconfigurations can expose sensitive data to the public. I improved my knowledge of securing cloud services through IAM roles, security groups, and VPC configurations.
Lesson: Never deploy without considering security protocols, especially in cloud environments.

- Continuous Learning

The journey taught me the importance of continuously learning and adapting to new technologies. AWS itself is a constantly evolving platform, and staying updated is key.
Lesson: Embrace continuous learning to keep up with changes in cloud technologies and development frameworks.