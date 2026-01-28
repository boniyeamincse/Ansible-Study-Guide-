# Zero to Hero: The Complete Ansible Study Guide 🚀

**Author:** AI Assistant  
**Version:** 1.0  
**License:** Open Source  

---

## 📖 Table of Contents
1. [Introduction to Ansible](#module-1-introduction-to-ansible)
2. [Inventory & Configuration](#module-2-inventory--configuration)
3. [Ad-Hoc Commands](#module-3-ad-hoc-commands)
4. [Playbook Fundamentals](#module-4-playbook-fundamentals)
5. [Variables & Facts](#module-5-variables--facts)
6. [Conditionals, Loops, & Handlers](#module-6-conditionals-loops--handlers)
7. [Modularizing with Roles](#module-7-modularizing-with-roles)
8. [Secrets Management (Ansible Vault)](#module-8-secrets-management)
9. [Final Project: Web Server Deployment](#final-project-web-server-deployment)

---

## 📂 Suggested Folder Structure

For a clean workflow, we recommend organizing your Ansible project like this:

```text
ansible-project/
├── ansible.cfg          # Main configuration file
├── inventory/
│   ├── dev.ini          # Development inventory
│   └── prod.ini         # Production inventory
├── playbooks/
│   ├── site.yml         # Master playbook
│   └── webserver.yml    # Webserver specific playbook
├── roles/               # Reusable roles
│   └── common/
│   └── nginx/
└── group_vars/          # Variables for groups
    ├── all.yml
    └── webservers.yml
```

---

# Module 1: Introduction to Ansible

### 🎯 Objectives
- Understand what Ansible is and why we use it.
- Learn the architecture (Controller vs. Managed Nodes).
- Install Ansible and verify the installation.

### 1.1 What is Ansible?
Ansible is an open-source, **agentless** IT automation tool. It handles configuration management, application deployment, and cloud provisioning.
- **Agentless:** You don't need to install software on the remote servers; it uses SSH.
- **Declarative:** You describe *what* you want (e.g., "Nginx should be installed"), and Ansible handles the *how*.

### 1.2 Architecture
- **Control Node:** The machine where you run Ansible (your laptop or a jump server). Linux/macOS preferred; WSL works on Windows.
- **Managed Nodes:** The servers you automate.
- **Inventory:** A list of managed nodes.

### 🧪 Lab 1: Installation & Setup
**Goal:** Install Ansible and verify it works.

#### Step 1: Install Ansible
On Ubuntu/Debian Control Node:
```bash
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
```

#### Step 2: Verify Installation
Check the version to ensure it's installed.
```bash
ansible --version
```

#### Step 3: Architecture Check
Ansible uses Python. Ensure Python is installed on the managed nodes (it usually is by default).

---

# Module 2: Inventory & Configuration

### 🎯 Objectives
- Define your servers in an Inventory file.
- Configure default behaviors using `ansible.cfg`.

### 2.1 The Inventory File
The inventory acts as the database of your servers. It can be a simple text file (INI format) or YAML.

**Example `inventory.ini`:**
```ini
[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com

[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

### 2.2 ansible.cfg
This file lets you set defaults so you don't have to type flags like `-i inventory.ini` every time.

**Example `ansible.cfg`:**
```ini
[defaults]
inventory = ./inventory.ini
remote_user = ubuntu
host_key_checking = False
```

### 🧪 Lab 2: Connecting to Nodes
**Goal:** Create an inventory and ping requirements.

#### Step 1: Create the Inventory
Create a file named `inventory.ini`.
```bash
echo "[local]" > inventory.ini
echo "localhost ansible_connection=local" >> inventory.ini
```
*> **Note:** We are using `localhost` for testing so you don't need external servers yet.*

#### Step 2: Test Connection
Use the `ping` module. This isn't an ICMP ping; it checks if Ansible can login and find Python.
```bash
ansible all -i inventory.ini -m ping
```

**Expected Output:**
```json
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

# Module 3: Ad-Hoc Commands

### 🎯 Objectives
- Run quick, one-off tasks without writing a playbook.
- Understand the syntax: `ansible <group> -m <module> -a <arguments>`.

### 3.1 Common Modules
- **ping:** Test connection.
- **shell / command:** Run shell commands.
- **package (apt/yum):** Manage packages.
- **service:** Manage services (start/stop).
- **copy:** Copy files.

### 🧪 Lab 3: Quick Tasks
**Goal:** Install a package and check uptime using ad-hoc commands.

#### Step 1: Check Disk Usage
Using the `shell` module.
```bash
ansible all -i inventory.ini -m shell -a "df -h"
```

#### Step 2: Check Uptime
```bash
ansible all -i inventory.ini -m command -a "uptime"
```
*> **Tip:** Ad-hoc commands are great for quick checks but bad for documentation. Use Playbooks for repeatable work.*

---

# Module 4: Playbook Fundamentals

### 🎯 Objectives
- Write your first Playbook in YAML.
- Understand the structure: Plays, Tasks, and Modules.
- Learn idempotency (running it twice doesn't break things).

### 4.1 YAML Syntax
- Use spaces, NOT tabs.
- Lists use `-`.
- Dictionaries use `key: value`.

### 4.2 Playbook Structure
```yaml
---
- name: This is the Play
  hosts: webservers  # Where does it run?
  become: yes        # Run as root (sudo)?
  
  tasks:
    - name: This is a Task
      package:       # The module
        name: nginx
        state: present
```

### 🧪 Lab 4: Your First Playbook
**Goal:** Automate Nginx installation on `localhost`.

#### Step 1: Create `site.yml`
```yaml
---
- name: Setup Web Server
  hosts: local
  connection: local  # Only needed because we are testing on localhost
  become: yes        # Requires sudo password if not passwordless
  
  tasks:
    - name: Install Nginx
      package:
        name: nginx
        state: present

    - name: Ensure Nginx is running
      service:
        name: nginx
        state: started
        enabled: yes
```

#### Step 2: Run the Playbook
```bash
ansible-playbook -i inventory.ini site.yml
```

#### Step 3: Verify Idempotency
Run the command again. You should see `changed=0` for the installation task, proving Ansible knows it's already there.

---

# Module 5: Variables & Facts

### 🎯 Objectives
- Make playbooks dynamic using variables.
- Understand "Facts" (system info discovered by Ansible).

### 5.1 Defining Variables
Variables can be defined in Playbooks, Inventory, or `group_vars`.

**In Playbook:**
```yaml
vars:
  http_port: 80
  server_name: my_app
```

### 5.2 Ansible Facts
Ansible automatically runs the `setup` module to gather info (IP, OS, CPU, etc.).
- Access them via `{{ ansible_facts['distribution'] }}` (e.g., "Ubuntu").

### 🧪 Lab 5: Using Variables
**Goal:** Create a user with a specific name defined in a variable.

#### Step 1: Create `users.yml`
```yaml
---
- name: Manage Users
  hosts: local
  connection: local
  become: yes
  vars:
    user_name: "jdoe"

  tasks:
    - name: Create User
      user:
        name: "{{ user_name }}"
        state: present
        shell: /bin/bash

    - name: Print OS Info
      debug:
        msg: "Running on {{ ansible_facts['distribution'] }}"
```

#### Step 2: Run it
```bash
ansible-playbook -i inventory.ini users.yml
```

---

# Module 6: Conditionals, Loops, & Handlers

### 🎯 Objectives
- Control execution flow (`when`).
- Iterate over items (`loop`).
- Restart services only when needed (`handlers`).

### 6.1 Handlers
Handlers are special tasks that run **only when notified** by another task (e.g., restart Nginx only if the config changed).

### 🧪 Lab 6: Advanced Control Flow
**Goal:** Install multiple packages and configure a file with a handler.

#### Step 1: Create `web_config.yml`
```yaml
---
- name: Web Server Config
  hosts: local
  connection: local
  become: yes

  tasks:
    - name: Install Essential Packages
      package:
        name: "{{ item }}"
        state: present
      loop:
        - git
        - curl
        - nginx

    - name: Copy Config File
      copy:
        content: "server_tokens off;"
        dest: /etc/nginx/conf.d/security.conf
      notify: Restart Nginx

  handlers:
    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```

#### Step 2: Run and Observe
Run it once. You see "Restart Nginx" run. Run it again. The config hasn't changed, so the Handler **skips**.

---

# Module 7: Modularizing with Roles

### 🎯 Objectives
- Organize complex playbooks into Roles.
- Use `ansible-galaxy`.

### 7.1 Role Structure
Roles separate tasks, vars, and handlers into files.
```text
roles/
  webserver/
    tasks/main.yml
    handlers/main.yml
    vars/main.yml
```

### 🧪 Lab 7: Creating a Role
**Goal:** Move our Nginx logic into a role.

#### Step 1: Initialize Role
```bash
mkdir roles
ansible-galaxy init roles/webserver --offline
```

#### Step 2: Move Tasks
Edit `roles/webserver/tasks/main.yml` and paste the Nginx installation tasks there (without the `tasks:` keyword).

#### Step 3: Use the Role in a Playbook
Create `site_role.yml`:
```yaml
---
- name: Deploy using Roles
  hosts: local
  connection: local
  become: yes
  
  roles:
    - webserver
```

---

# Module 8: Secrets Management (Ansible Vault)

### 🎯 Objectives
- Encrypt sensitive data (passwords, keys).

### 8.1 Basic Commands
- `ansible-vault create secret.yml`
- `ansible-vault edit secret.yml`
- `ansible-vault encrypt existing_file.yml`

### 🧪 Lab 8: Encrypting a Variable
**Goal:** Create an encrypted variable for a database password.

#### Step 1: Create Encrypted File
```bash
ansible-vault create secrets.yml
# Enter a password (e.g., 'password123')
# Add content:
# db_password: SuperSecretPassword!
```

#### Step 2: Use in Playbook
```yaml
---
- name: Use Secrets
  hosts: local
  connection: local
  vars_files:
    - secrets.yml
  
  tasks:
    - name: Show Secret (Don't do this in production)
      debug:
        msg: "The password is {{ db_password }}"
```

#### Step 3: Run with Vault Password
```bash
ansible-playbook -i inventory.ini site_secrets.yml --ask-vault-pass
```

---

# Final Project: Web Server Deployment

### 🎯 Scenario
You need to deploy a custom HTML page to a web server.
Requirements:
1. Install Nginx.
2. Remove the default `index.html`.
3. Deploy a custom `index.html` "Hello from Ansible".
4. Ensure the service is running.

### 📂 Solution Files

**1. inventory.ini**
```ini
[web]
localhost ansible_connection=local
```

**2. deploy_web.yml**
```yaml
---
- name: Final Project - Web Deployment
  hosts: web
  become: yes
  vars:
    web_content: "<h1>Hello from Ansible Automation! 🚀</h1>"

  tasks:
    - name: Install Nginx
      package:
        name: nginx
        state: present

    - name: Update Index Page
      copy:
        content: "{{ web_content }}"
        dest: /var/www/html/index.html
        mode: '0644'
      notify: Restart Nginx

    - name: Ensure Service Running
      service:
        name: nginx
        state: started
        enabled: yes

  handlers:
    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```

### 🚀 Execution
Run the project:
```bash
ansible-playbook -i inventory.ini deploy_web.yml
```

**Verify:**
Open your browser or use curl:
```bash
curl http://localhost
```

---
**Congratulations!** You have completed the Ansible Study Guide. 🎓
