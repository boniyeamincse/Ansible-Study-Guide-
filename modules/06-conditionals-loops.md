# Module 6: Conditionals & Loops

## 📌 Topics
- **Conditionals (`when`):** Run tasks only if a condition is met (e.g., `when: ansible_os_family == "Debian"`).
- **Loops (`loop`):** Repeat a task for a list of items.
- **Register:** Save the output of a module to a variable for later use.

---

## 🧪 Labs

### Lab 6.1: Installing Multiple Packages with Loops
**Goal:** Instead of writing 5 tasks to install 5 packages, write 1 task that loops 5 times.

**Step 1: Create `playbooks/loops.yml`**
```yaml
---
- name: Loop Demo
  hosts: all
  become: yes
  
  tasks:
    - name: Install Common Tools
      package:
        name: "{{ item }}"
        state: present
      loop:
        - git
        - curl
        - htop
        - unzip
        - tree
```
*> **Note:** `{{ item }}` is a special variable that holds the current value of the loop.*

**Step 2: Run it**
```bash
ansible-playbook playbooks/loops.yml
```

---

### Lab 6.2: Loop over a List of Dictionaries
**Goal:** Create multiple users with specific IDs.

**Step 1: Create `playbooks/users_loop.yml`**
```yaml
---
- name: Create Users
  hosts: all
  become: yes

  tasks:
    - name: Ensure Users Exist
      user:
        name: "{{ item.username }}"
        uid: "{{ item.uid }}"
        state: present
      loop:
        - { username: 'dev_alice', uid: 2001 }
        - { username: 'dev_bob',   uid: 2002 }
```

**Step 2: Run and Verify**
```bash
ansible-playbook playbooks/users_loop.yml
```
Check if users exist:
```bash
ansible all -m shell -a "tail -n 2 /etc/passwd"
```

---

### Lab 6.3: Register Variables & Conditional Logic
**Goal:** Run a command, save ("register") the result, and decide what to do next based on that result.

**Scenario:** We want to restart a service only if a configuration file specific string is missing.

**Step 1: Create `playbooks/conditional_logic.yml`**
```yaml
---
- name: Conditional Demo
  hosts: all
  
  tasks:
    - name: Check if file /tmp/flag exists
      stat:
        path: /tmp/flag
      register: flag_file  # Save result to this variable

    - name: Print Message if File Exists
      debug:
        msg: "The flag file was found!"
      when: flag_file.stat.exists == true

    - name: Print Message if File is MISSING
      debug:
        msg: "No flag file found. Safe to proceed."
      when: flag_file.stat.exists == false
```

**Step 2: Run it (File missing)**
The second debug task should run.

**Step 3: Create the file and run again**
```bash
ansible all -m file -a "path=/tmp/flag state=touch"
```
Run playbook again. Now the first debug task should run.
