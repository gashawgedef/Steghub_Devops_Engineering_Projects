# Project 13: Ansible Dynamic Assignments (Include) and Community Roles
This project demonstrates the usage of dynamic assignments (using `include_tasks ` and `include_role`) and the integration of community roles from Ansible Galaxy to create a flexible, modular Ansible configuration.


Here’s a detailed README document formatted with Markdown headings, including the specified sections, descriptions, and example code snippets:

Project 13: Ansible Dynamic Assignments (Include) and Community Roles
This project demonstrates the usage of dynamic assignments (using include_tasks and include_role) and the integration of community roles from Ansible Galaxy to create a flexible, modular Ansible configuration.


## Introduction

Dynamic assignments in Ansible allow for conditional inclusion of tasks or roles, making configurations more modular and easier to manage. Community roles provide pre-built modules for common tasks, enabling you to leverage the Ansible community's expertise.

## Dynamic Assignments in Ansible
Dynamic assignments enhance playbooks by allowing conditional execution of tasks or roles based on variables or conditions defined at runtime.

### Benefits of Dynamic Assignments

- **Reusability:** Avoid redundant code by reusing task files and roles.
- **Conditional Execution:** Include tasks or roles based on specific criteria.
- **Modularity:** Break down complex playbooks into manageable parts.

Example of Dynamic Assignment in a Playbook

```
# playbooks/site.yml

- name: Load Balancers Assignment
  hosts: lb
  tasks:
    - name: Check if load balancer is required
      set_fact:
        load_balancer_is_required: true  # or some condition

    - name: Import Load Balancer tasks if required
      include_tasks: ../static-assignments/loadbalancers.yml
      when: load_balancer_is_required
```
In this example:

- The `include_tasks` module conditionally imports `loadbalancers.yml` if `load_balancer_is_required` is set to `true`.

## Community Roles

Ansible Galaxy offers community roles that provide pre-built modules for various services, allowing you to save time and ensure consistency in your configurations.

## Installing a Community Role

You can install community roles using the `ansible-galaxy` command:

```
ansible-galaxy install nginxinc.nginx
```

## Using Community Roles in a Playbook
Once installed, community roles can be included in your playbook as shown below:

```
# playbooks/webserver.yml

- name: Web Server Setup
  hosts: webservers
  roles:
    - { role: nginxinc.nginx, when: enable_nginx }
    - { role: apache, when: enable_apache }
```

