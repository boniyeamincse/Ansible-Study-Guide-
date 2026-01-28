# Module 4: Ansible Playbooks

## 📌 Topics
- **What is a Playbook?** A YAML file describing a set of tasks to run on hosts. Unlike ad-hoc commands, playbooks are saved, version-controlled, and repeatable.
- **Structure:**
  - `hosts`: Target machines.
  - `vars`: Variables for the play.
  - `tasks`: The list of actions.
- **Idempotency:** The concept that running a task multiple times should not change the result if the state is already correct.

---

## 🧪 Labs

### Lab 4.1: Install Apache Web Server
**Goal:** Write a playbook that installs the Apache web server (`apache2` on Ubuntu or `httpd` on CentOS).

**Step 1: Create the Playbook file**
```bash
mkdir playbooks
nano playbooks/install_apache.yml
```

**Step 2: Write the YAML Code**
Pay close attention to indentation (2 spaces).
```yaml
---
- name: Install Apache Web Server
  hosts: all
  become: yes  # We need sudo execution

  tasks:
    # Task 1: Update apt cache (Ubuntu only)
    - name: Update Apt Cache
      apt:
        update_cache: yes
      when: ansible_os_family == "Debian"

    # Task 2: Install Apache (Ubuntu)
    - name: Install Apache
      apt:
        name: apache2
        state: present
      when: ansible_os_family == "Debian"
```

**Step 3: Run the Playbook**
```bash
ansible-playbook playbooks/install_apache.yml
```
*You should see "changed" in yellow.*

---

### Lab 4.2: Deploy Website Content & Service State
**Goal:** Not just install software, but ensure it is running and serving our content.

**Step 1: Edit `playbooks/deploy_site.yml`**
```yaml
---
- name: Deploy Web Site
  hosts: all
  become: yes

  tasks:
    - name: Copy Index Page
      copy:
        content: "<h1>Ansible is Cool! 🚀</h1>"
        dest: /var/www/html/index.html

    - name: Ensure Apache is Started
      service:
        name: apache2
        state: started
        enabled: yes
```
*> **Tip:** `enabled: yes` means "start automatically when server reboots".*

**Step 2: Run it**
```bash
ansible-playbook playbooks/deploy_site.yml
```

---

### Lab 4.3: Debugging Tasks
**Goal:** Sometimes you need to see what's happening or print a variable during execution.

**Step 1: Create `playbooks/debug_demo.yml`**
```yaml
---
- name: Debug Demo
  hosts: localhost
  connection: local

  tasks:
    - name: Print a message
      debug:
        msg: "I am running on {{ ansible_hostname }}"
        
    - name: Print all known variables (Verbosity)
      debug:
        var: ansible_facts
      verbosity: 2  # Only shows if you run with -vv
```

**Step 2: Run normally**
```bash
ansible-playbook playbooks/debug_demo.yml
```
*You see the message.*

**Step 3: Run with increased verbosity**
```bash
ansible-playbook playbooks/debug_demo.yml -vv
```
*Now you see the distinct variable structure.*
