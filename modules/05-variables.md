# Module 5: Variables & Facts

## 📌 Topics
- **Variable Scope:** Global, Play, Host, Group.
- **Defining Vars:**
  - `vars:` section in playbook.
  - `group_vars/` and `host_vars/` directories.
  - `-e` (extra vars) at command line.
- **Facts:** System info gathered by Ansible (IP, Memory, OS).
- **set_fact:** Create variables dynamically during runtime.

---

## 🧪 Labs

### Lab 5.1: Gathering and Filtering Facts
**Goal:** Understand what information Ansible "knows" about your servers automatically.

**Step 1: Gather all facts**
Run the setup module. This runs automatically at the start of playbooks, but we can run it manually to see the data.
```bash
ansible all -m setup
```
*Result: A huge JSON output describing your system.*

**Step 2: Filter for specific info**
Let's find just the memory information.
```bash
ansible all -m setup -a "filter=ansible_memtotal_mb"
```

**Step 3: Find OS distribution**
```bash
ansible all -m setup -a "filter=ansible_distribution*"
```

---

### Lab 5.2: Using Variables in Playbooks
**Goal:** Write a playbook that uses variables instead of hard-coded values.

**Step 1: Create `playbooks/vars_demo.yml`**
```yaml
---
- name: Variable Demo
  hosts: all
  become: yes
  vars:
    # Defining variables here
    package_name: git
    file_path: /tmp/server_info.txt

  tasks:
    - name: Install Package
      package:
        name: "{{ package_name }}"  # Using the variable
        state: present

    - name: Write Fact to File
      copy:
        # Using a Fact (system info) and a Variable
        content: "Host {{ ansible_hostname }} has {{ ansible_memtotal_mb }}MB RAM."
        dest: "{{ file_path }}"
```

**Step 2: Run the playbook**
```bash
ansible-playbook playbooks/vars_demo.yml
```

**Step 3: Verify**
Check the content of the created file.
```bash
ansible all -m shell -a "cat /tmp/server_info.txt"
```

---

### Lab 5.3: Using Group Variables (`group_vars`)
**Goal:** Apply different settings to different groups of servers (e.g., Port 80 for Web, Port 3306 for DB).

**Step 1: Create the directory structure**
Ansible looks for a folder named `group_vars` relative to your inventory or playbook.
```bash
mkdir group_vars
```

**Step 2: Create variables for `webservers`**
Create a file named precisely after the group in your inventory.
```bash
nano group_vars/webservers.yml
```
Content:
```yaml
http_port: 80
service_name: nginx
```

**Step 3: Create variables for `dbservers`**
```bash
nano group_vars/dbservers.yml
```
Content:
```yaml
db_port: 3306
service_name: mysql
```

**Step 4: Test with a Debug Playbook**
Create `playbooks/group_vars_test.yml`:
```yaml
---
- name: Test Group Vars
  hosts: all
  tasks:
    - name: Show Port
      debug:
        msg: "I am {{ ansible_hostname }} and my service is {{ service_name }}"
```
*Run it and observe how the output changes depending on which group the server belongs to.*
