# Module 1: Introduction to Ansible

## 📌 Topics
- **What is Ansible?** 
  - An open-source, **agentless** automation tool for configuration management, app deployment, and orchestration.
  - **Why use it?** Simple (YAML), powerful, and agentless (uses SSH).
- **Architecture:**
  - **Control Node:** The machine running Ansible.
  - **Managed Nodes:** The target servers.
  - **Inventory:** List of managed nodes.
  - **Modules:** Small programs Ansible pushes to nodes to do work (e.g., `apt`, `service`).
- **Installation:**
  - Supported on Linux, macOS, WSL (Windows).

---

## 🧪 Labs

### Lab 1.1: Install Ansible
**Goal:** Install Ansible on your Control Node.

**Steps:**
1. **Update Repositories:**
   ```bash
   sudo apt update
   ```
2. **Install Ansible:**
   ```bash
   sudo apt install ansible -y
   ```
   *(For RHEL/CentOS: `sudo yum install ansible`)*

### Lab 1.2: Verify Installation
**Goal:** Check if Ansible is ready.

**Steps:**
1. **Check Version:**
   ```bash
   ansible --version
   ```
   *Expected: Output showing version (e.g., 2.10+) and python version.*

### Lab 1.3: Creating Your First Inventory
**Goal:** Create a simple file to define your hosts.

**Steps:**
1. **Create a file named `inventory.ini`:**
   ```ini
   [local]
   localhost ansible_connection=local
   ```
2. **Ping the node:**
   ```bash
   ansible all -i inventory.ini -m ping
   ```
   *Note: `-m ping` uses the ping module (not ICMP).*

**Expected Output:**
```json
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
