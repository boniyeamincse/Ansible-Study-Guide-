# Module 2: Ansible Inventory & Configuration

## 📌 Topics
- **Static vs Dynamic Inventory:**
  - **Static:** Text files (INI/YAML) listing hosts.
  - **Dynamic:** Python scripts calling cloud APIs (AWS, Azure) to get hosts.
- **Inventory Groups:**
  - Organize hosts by role (e.g., `[webservers]`, `[dbservers]`).
- **ansible.cfg:**
  - Central configuration file to set defaults (inventory path, user, private keys).

---

## 🧪 Labs

### Lab 2.1: Create Multi-Host Inventory
**Goal:** Learn how to group servers by their function (e.g., Web Servers, Database Servers) so you can target them separately.

**Scenario:** Imagine you have 3 servers. Two are for your website (Nginx) and one is for your database (MySQL).

**Step 1: Create or Edit `inventory.ini`**
Open the file in your text editor:
```bash
nano inventory.ini
```

**Step 2: Define your Groups**
Add the following content.
*> **Note:** If you don't have real servers, you can use `localhost` aliases to simulate them.*

```ini
# These are our Web Servers
[webservers]
web1.example.com
web2.example.com

# This is our Database Server
[dbservers]
db1.example.com

# Group of groups (Optional advanced topic)
[production:children]
webservers
dbservers

# Variables for all hosts
[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/my_key.pem
```

---

### Lab 2.2: Organize Hosts and Test Connectivity
**Goal:** Verify that you can target specific groups of servers independently.

**Step 1: Ping only the Web Servers**
We want to check if the web servers are online, ignoring the database servers.
```bash
ansible webservers -i inventory.ini -m ping
```
*Result: You should see generic output (or errors if these domains don't exist), but only for `web1` and `web2`.*

**Step 2: Ping only the Database Servers**
```bash
ansible dbservers -i inventory.ini -m ping
```

**Step 3: List all hosts**
Use the `--list-hosts` flag to see who Ansible *would* talk to, without actually connecting.
```bash
ansible all -i inventory.ini --list-hosts
```
**Expected Output:**
```text
  hosts (3):
    web1.example.com
    web2.example.com
    db1.example.com
```

---

### Lab 2.3: Override Configuration with `ansible.cfg`
**Goal:** Save time by setting default options so you don't have to type `-i inventory.ini` or private key paths every time.

**Step 1: Create the `ansible.cfg` file**
This file must be in the directory where you run your commands (current directory).
```bash
nano ansible.cfg
```

**Step 2: Add Default Settings**
Paste this configuration:
```ini
[defaults]
# Use this inventory file automatically
inventory = ./inventory.ini

# Don't ask to verify SSH fingerprints (useful for labs, bad for production)
host_key_checking = False

# Suppress warnings about Python versions
deprecation_warnings = False
```

**Step 3: Verify the New Default**
Run the ping command *without* specifying the inventory file.
```bash
ansible all -m ping
```
*Did it work? If yes, Ansible successfully read your `ansible.cfg`.*
