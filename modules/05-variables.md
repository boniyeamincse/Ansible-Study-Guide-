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

### Lab 5.1: Gathering Facts
**Goal:** See what Ansible knows about your hosts.

**Command:**
```bash
ansible all -m setup
```
**Filter output:**
```bash
ansible all -m setup -a "filter=ansible_memtotal_mb"
```

### Lab 5.2: Using Variables in Playbooks
**File:** `playbooks/vars_demo.yml`
```yaml
---
- name: Variable Demo
  hosts: all
  vars:
    package_name: git
    file_path: /tmp/info.txt

  tasks:
    - name: Install Package
      package:
        name: "{{ package_name }}"
        state: present
      become: yes

    - name: Write Fact to File
      copy:
        content: "Host {{ ansible_hostname }} has {{ ansible_memtotal_mb }}MB RAM."
        dest: "{{ file_path }}"
```

### Lab 5.3: Group Vars
**Goal:** Set separate variables for groups.

1. Create `group_vars/webservers.yml`:
   ```yaml
   http_port: 80
   ```
2. Create `group_vars/dbservers.yml`:
   ```yaml
   db_port: 3306
   ```
3. Ansble automatically loads these when targeting respective groups.
