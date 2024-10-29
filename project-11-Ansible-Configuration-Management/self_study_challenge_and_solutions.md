
## What is configuration management?

**Configuration management** is a process for maintaining a desired state of IT systems and components. It helps ensure that a system consistently performs as expected throughout its lifecycle.

System administrators can use configuration management tools to set up an IT system, such as a server or workstation, then build and maintain other servers and workstations with the same settings. They can also use configuration assessments and drift analyses to continuously identify systems that have strayed from the desired state and need to be updated, reconfigured, or patched.

As a part of **IT Service Management (ITSM)** process, configuration management databases (CMDBs) track individual configuration items (CIs): any asset or component involved in the delivery of IT services. CMDBs store information about a CI’s attributes, dependencies, and changes to its configuration over time—enabling IT teams to map and maintain the relationships that tie CIs together.

## Automating configuration management

**Automating configuration management** is essential to establishing a reliable, consistent, and well-maintained IT environment at scale. Rather than relying on individuals to perform time-consuming manual configuration tasks, automation allows teams to consistently deploy and decommission infrastructure components in less time, with fewer opportunities for human mistakes. It also makes it possible to maintain consistent system settings across datacenter, cloud, and edge environments for an application’s entire life cycle, minimizing both performance and security issues.

Automation can help enterprises reduce costs, complexity, and manual errors in a variety of IT use cases:

- **Infrastructure automation**: configure and manage server infrastructure to enforce consistency and eliminate configuration drift.

- **Cloud automation**: configure and manage cloud resources including operating systems, security groups, load balancers, and virtual private clouds. 

- **Network automation**: configure and manage network devices such routers and switches. 

- **Security automation**: configure and manage security devices such as firewalls and intrusion detection systems— and apply consistent network access policies. 

- **Edge automation**: configure and manage remote infrastructure systems including network, security, IoT devices, and server equipment.

 ## Jump Servers

A **jump server**, **jump host** or **jump box** is a system on a network used to access and manage devices in a separate security zone. A jump server is a hardened and monitored device that spans two dissimilar security zones and provides a controlled means of access between them. The most common example is managing a host in a DMZ from trusted networks or computers.

### Implementation
Jump servers are often placed between a secure zone and a DMZ to provide transparent management of devices on the DMZ once a management session has been established. The jump server acts as a single audit point for traffic and also a single place where user accounts can be managed. A prospective administrator must log into the jump server in order to gain access to the DMZ assets and all access can be logged for later audit.





