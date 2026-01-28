# Module 6: Conditionals & Loops

## 📌 Topics
- **Conditionals (`when`):** Run tasks only if a condition is met (e.g., `when: ansible_os_family == "Debian"`).
- **Loops (`loop`):** Repeat a task for a list of items.
- **Register:** Save the output of a module to a variable for later use.

---

## 🧪 Labs

### Lab 6.1: Installing Multiple Packages (Loops)
**Goal:** Install a list of tools efficiently.

**File:** `playbooks/loops.yml`
```yaml
---
- name: Install Tools
  hosts: all
  become: yes
  
  tasks:
    - name: Install List of Packages
      package:
        name: "{{ item }}"
        state: present
      loop:
        - git
        - curl
        - htop
        - vim
```

### Lab 6.2: Create Multiple Users
**Goal:** Create users from a dictionary loop.

```yaml
    - name: Create Users
      user:
        name: "{{ item.name }}"
        uid: "{{ item.uid }}"
        state: present
      loop:
        - { name: 'alice', uid: 1050 }
        - { name: 'bob', uid: 1051 }
```

### Lab 6.3: Register & Conditionals
**Goal:** Run a command, check output, run next task based on it.

```yaml
    - name: Check if File Exists
      stat:
        path: /tmp/lockfile
      register: file_data

    - name: Run only if Locked
      debug:
        msg: "System is locked!"
      when: file_data.stat.exists
```
