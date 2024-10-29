# For better understanding or Ansible-artifacts-re-use –Code Refactoring
# Ansible Artifacts Reuse and Code Refactoring

Reusing Ansible artifacts and refactoring code involves organizing your Ansible playbooks, roles, and modules to be more modular, maintainable, and reusable. Here are some key concepts and practices to help you with this:

## 1. Modular Playbooks
Break down your playbooks into smaller, modular tasks. This allows you to reuse these tasks across different playbooks and roles.

### Example:
Instead of having a single large playbook, create smaller playbooks for specific tasks:
```yaml
# main.yml
- name: Main playbook
  hosts: all
  roles:
    - common
    - webserver
    - database

# common.yml
- name: Common setup
  hosts: all
  tasks:
    - name: Update apt cache
      apt: update_cache=yes

# webserver.yml
- name: Webserver setup
  hosts: webservers
  tasks:
    - name: Install Nginx
      apt: name=nginx state=present

# database.yml
- name: Database setup
  hosts: dbservers
  tasks:
    - name: Install MySQL
      apt: name=mysql-server state=present

```

2. Roles
Use Ansible roles to organize your tasks, handlers, variables, and files. Roles are a way to package and reuse configurations.

Example:
Directory structure for a role named webserver:
```
webserver/
├── tasks
│   └── main.yml
├── handlers
│   └── main.yml
├── templates
│   └── nginx.conf.j2
├── files
│   └── index.html
├── vars
│   └── main.yml
├── defaults
│   └── main.yml
├── meta
│   └── main.yml

```
3. Role Dependencies
Define dependencies between roles in the meta/main.yml file of a role. This helps in reusing and organizing complex configurations.

Example:
# roles/webserver/meta/main.yml
dependencies:
  - { role: common, some_var: some_value }
  - { role: database }
4. Shared Variables
Use group variables (group_vars) and host variables (host_vars) to manage variables shared across different playbooks and roles.

Example:
group_vars/
└── all.yml

host_vars/
└── webserver.yml

5. Dynamic Includes
Use include_tasks and import_tasks to dynamically include tasks based on conditions. This helps in reusing tasks in different scenarios.

Example:

- name: Include OS-specific tasks
  include_tasks: "{{ item }}"
  with_items:
    - "tasks/{{ ansible_os_family }}.yml"
  when: ansible_os_family in ['Debian', 'RedHat']
6. Ansible Galaxy
Leverage Ansible Galaxy to use and share roles. This can significantly reduce the time required to write common tasks from scratch.

Example:
Install a role from Ansible Galaxy:

```
ansible-galaxy install geerlingguy.nginx
```

7. Code Refactoring Practices

- DRY (Don't Repeat Yourself): Avoid duplicating code by creating reusable roles and tasks.
- Naming Conventions: Use consistent naming conventions for tasks, variables, and roles.
- Documentation: Document your playbooks and roles to make them easier to understand and maintain.
- Version Control: Use version control systems like Git to manage changes and collaborate with others.
8. Example Refactored Playbook
Refactored version of a playbook that installs and configures a web server:

```
# site.yml
- hosts: all
  roles:
    - common
    - webserver

# roles/common/tasks/main.yml
- name: Update apt cache
  apt: update_cache=yes

# roles/webserver/tasks/main.yml
- name: Install Nginx
  apt: name=nginx state=present

- name: Copy Nginx config
  template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf

- name: Start Nginx
  service: name=nginx state=started enabled=yes
```
By organizing your Ansible artifacts in a modular and reusable manner, you can greatly improve the maintainability and scalability of your automation code.