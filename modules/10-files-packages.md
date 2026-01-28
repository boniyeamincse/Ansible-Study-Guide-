# Module 10: File & Package Management

## 📌 Topics
- **File Management:**
  - `file`: Create directories, symlinks, change permissions/ownership.
  - `copy`: Simple file transfer.
  - `fetch`: Copy FROM remote TO control node (backup).
- **Package Management:**
  - `package`: Generic module (auto-detects apt/yum).
  - `pip`: Install Python libraries.
  - `npm`: Install Node.js packages.

---

## 🧪 Labs

### Lab 10.1: Managing Directory Trees & Symlinks
**Goal:** Create a specific log folder structure and link it to the user's home directory.

**Step 1: Create `playbooks/manage_files.yml`**
```yaml
---
- name: File Management
  hosts: all
  become: yes

  tasks:
    - name: Create Log Directory
      file:
        path: /var/log/my_app_logs
        state: directory
        mode: '0755'
        owner: www-data
        group: www-data

    - name: Create a Symbolic Link
      file:
        src: /var/log/my_app_logs
        dest: /home/ubuntu/app_logs
        state: link
```

**Step 2: Verification**
Run the playbook, then check the file system:
```bash
ls -l /home/ubuntu/app_logs
```

---

### Lab 10.2: Installing Specific Software Versions
**Goal:** Install a specific version of a Python library using pip.

**Step 1: Install Pip first**
We need `pip` installed before we can use the `pip` module.

**Step 2: Create `playbooks/install_python_lib.yml`**
```yaml
---
- name: Install Python Libs
  hosts: all
  become: yes

  tasks:
    - name: Install Pip3
      package:
        name: python3-pip
        state: present

    - name: Install Requests Library v2.25.1
      pip:
        name: requests
        version: 2.25.1
```

**Step 3: Run it**
```bash
ansible-playbook playbooks/install_python_lib.yml
```
