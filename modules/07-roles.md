# Module 7: Roles & Ansible Galaxy

## 📌 Topics
- **Roles:** The standard way to package Ansible automation. Separation of concerns (Vars in one folder, Tasks in another).
- **Directory Structure:**
  - `roles/myrole/tasks/main.yml`
  - `roles/myrole/vars/main.yml`
  - `roles/myrole/templates/`
- **Ansible Galaxy:** A public hub for sharing roles (`ansible-galaxy` command).

---

## 🧪 Labs

### Lab 7.1: Create a Custom Nginx Role
**Goal:** Refactor a messy playbook into a clean structure using Roles.

**Step 1: Create the directory structure**
Use the `ansible-galaxy init` command to create the skeleton.
```bash
mkdir roles
cd roles
ansible-galaxy init webserver
cd ..
```
*This creates folders like `roles/webserver/tasks`, `defaults`, `vars`, etc.*

**Step 2: Add the Tasks**
Edit `roles/webserver/tasks/main.yml`. Note that we **do not** start with `- hosts:`. We just list tasks.
```yaml
---
- name: Install Nginx
  package:
    name: nginx
    state: present

- name: Start Nginx
  service:
    name: nginx
    state: started
```

**Step 3: Use the Role in a Playbook**
Create a master playbook `site_roles.yml`:
```yaml
---
- name: Deploy Web Servers using Roles
  hosts: webservers  # Target specific group
  become: yes
  
  roles:
    - webserver
```

**Step 4: Run it**
```bash
ansible-playbook site_roles.yml
```

---

### Lab 7.2: Installing Roles from Ansible Galaxy
**Goal:** Don't reinvent the wheel. Download a community role to install MySQL.

**Step 1: Install the role**
We will install a popular role by "geerlingguy".
```bash
ansible-galaxy install geerlingguy.mysql
```
*The role is downloaded to `~/.ansible/roles` or your local `roles/` folder.*

**Step 2: Create a playbook to use it**
Create `mysql_deploy.yml`:
```yaml
---
- name: Database Server Setup
  hosts: dbservers
  become: yes
  vars:
    mysql_root_password: "SuperSecretPassword123"
  
  roles:
    - geerlingguy.mysql
```
*> **Note:** We passed a variable `mysql_root_password` which the role uses to configure the DB.*

**Step 3: Run it**
```bash
ansible-playbook mysql_deploy.yml
```
