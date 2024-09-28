# MEAN Stack Project Documentation

## Project Overview


## 1. Refreshing Knowledge of the OSI Model

### Key Learnings:
- **Definition:** The OSI (Open Systems Interconnection) model is a conceptual framework used to understand network communication.
- **Seven Layers:**
  1. **Physical Layer:** Deals with the physical connection between devices (cables, switches).
  2. **Data Link Layer:** Responsible for node-to-node data transfer (Ethernet).
  3. **Network Layer:** Manages data routing and forwarding (IP).
  4. **Transport Layer:** Ensures reliable data transmission (TCP/UDP).
  5. **Session Layer:** Manages sessions between applications (establishing, maintaining, and terminating sessions).
  6. **Presentation Layer:** Translates data between the application layer and the network (encryption, compression).
  7. **Application Layer:** Closest to the end-user, where applications interact with the network (HTTP, FTP).

## 2. Load Balancing in AWS

### Key Learnings:
- **Definition:** Load balancing distributes incoming traffic across multiple targets (e.g., EC2 instances) to ensure reliability and performance.
- **Types of Load Balancers in AWS:**
  - **Application Load Balancer (ALB):** Best for HTTP and HTTPS traffic; provides advanced routing features.
  - **Network Load Balancer (NLB):** Best for TCP traffic; capable of handling millions of requests per second.
  - **Classic Load Balancer (CLB):** Older version that supports both TCP and HTTP traffic.


## 3. HTML, CSS, JavaScript, and MEAN Stack Development

### Key Learnings:
- **HTML:** Structure of the web application; created forms, tables, and layouts.
- **CSS:** Styling the application to enhance user experience; used Flexbox and Grid for layout designs.
- **JavaScript:** Added interactivity to the web application; utilized ES6 features like arrow functions and template literals.

### MEAN Stack Components:
- **MongoDB:** NoSQL database; used for data storage and retrieval.
- **Express.js:** Web framework for Node.js; handled API requests and middleware.
- **Angular:** Frontend framework; built dynamic single-page applications with components and services.
- **Node.js:** JavaScript runtime; served the backend logic.

### Implementation Steps:
1. Set up the project structure.
2. Created the backend RESTful APIs using Express and connected to MongoDB.
3. Developed the frontend with Angular, utilizing services to communicate with the backend.
4. Implemented routing in Angular for navigation within the application.



## Challenges Faced:
- The commands used to start and check status  for **mongodb** was not working  and I changed to another one

- The port number **3300** was not working and I changed it to **3000** and it works correctly.

## Conclusion
This project allowed me to consolidate my understanding of the OSI model, gain hands-on experience with load balancing in AWS, and develop a full-stack application using the MEAN stack. Each component played a vital role in the overall architecture and performance of the application.

### Continuous Learning

This journey reinforced the importance of continuous learning and adapting to new technologies. AWS is a constantly evolving platform, and staying up-to-date with new services and best practices is key to success.
Lesson: Embrace continuous learning to keep pace with changes in cloud technologies and development frameworks.