# Module 3: Ansible Ad-Hoc Commands

## 📌 Topics
- **Ad-Hoc Commands:** Quick, one-liner commands for simple tasks.
- **Syntax:** `ansible <pattern> -m <module> -a "<arguments>"`
- **Common Modules:**
  - `ping`: Connectivity check.
  - `command` / `shell`: Run shell commands.
  - `copy`: Copy files.
  - `file`: Manage files/dirs/permissions.
  - `user`: Manage user accounts.
  - `service`: Start/Stop services.

---

## 🧪 Labs

### Lab 3.1: Ping All Servers
**Steps:**
```bash
ansible all -m ping
```

### Lab 3.2: Create a User
**Goal:** Create a user `deployer` on all nodes.

**Steps:**
```bash
ansible all -m user -a "name=deployer state=present shell=/bin/bash" --become
```
*Note: `--become` is used to elevate execution (sudo).*

### Lab 3.3: Copy a File
**Goal:** Copy a config file to `/tmp`.

**Steps:**
1. **Create a dummy file:**
   ```bash
   echo "Setting = true" > app.conf
   ```
2. **Copy it:**
   ```bash
   ansible all -m copy -a "src=app.conf dest=/tmp/app.conf"
   ```

### Lab 3.4: Change Permissions
**Goal:** Ensure a directory allows write access.

**Steps:**
```bash
ansible all -m file -a "path=/tmp/app.conf mode=0644 owner=root group=root" --become
```
