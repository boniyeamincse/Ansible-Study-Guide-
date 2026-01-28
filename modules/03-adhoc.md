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
**Goal:** Quickest way to check if your fleet is online.
```bash
ansible all -m ping
```

---

### Lab 3.2: Create a User Account
**Goal:** Create a user named `deployer` on all servers using the `user` module.

**Step 1: Run the command**
We need to use `--become` because creating users requires root/sudo privileges.
```bash
ansible all -m user -a "name=deployer state=present shell=/bin/bash" --become
```

**Step 2: Verify it worked**
We can run a shell command on the remote server to check the `/etc/passwd` file.
```bash
ansible all -m shell -a "grep deployer /etc/passwd"
```
**Expected Output:**
`deployer:x:1001:1001::/home/deployer:/bin/bash` (numbers may vary)

---

### Lab 3.3: Copy a File to Multiple Hosts
**Goal:** Push a configuration file from your laptop (Control Node) to all remote servers.

**Step 1: Create a source file**
Create a dummy file locally.
```bash
echo "Welcome to my server" > banner.txt
```

**Step 2: Use the `copy` module**
We specify `src` (where it is now) and `dest` (where it should go).
```bash
ansible all -m copy -a "src=banner.txt dest=/tmp/banner.txt"
```

**Step 3: Verify the file exists remotely**
```bash
ansible all -m command -a "cat /tmp/banner.txt"
```

---

### Lab 3.4: Change File Permissions
**Goal:** Ensure a specific file allows read/write access.

**Step 1: Use the `file` module**
We want to set the mode to `0644` (Read/Write for owner, Read for everyone else) and clear ownership.
```bash
ansible all -m file -a "path=/tmp/banner.txt mode=0644 owner=root group=root" --become
```
**Command Breakdown:**
- `path`: The file to touch.
- `mode`: Permission bits.
- `owner/group`: Who owns it.
- `--become`: Run as sudo (required since we are setting owner to root).
