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
**Goal:** specific groups for servers.

**Steps:**
1. **Create `inventory.ini`:**
   ```ini
   [webservers]
   web1.example.com
   web2.example.com

   [dbservers]
   db1.example.com

   [all:vars]
   ansible_user=ubuntu
   ansible_ssh_private_key_file=~/.ssh/test_key.pem
   ```
   *(Replace domains with IPs of your lab VMs or use localhost lines if testing locally)*

### Lab 2.2: Organize Hosts and Test
**Goal:** Ping specific groups.

**Steps:**
1. **Ping only webservers:**
   ```bash
   ansible webservers -i inventory.ini -m ping
   ```

### Lab 2.3: Override Config with ansible.cfg
**Goal:** Avoid typing `-i inventory.ini` every time.

**Steps:**
1. **Create `ansible.cfg` in the current directory:**
   ```ini
   [defaults]
   inventory = ./inventory.ini
   host_key_checking = False
   deprecation_warnings = False
   ```
2. **Test without `-i`:**
   ```bash
   ansible all -m ping
   ```
