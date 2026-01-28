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

### Lab 1.1: Install Ansible on Control Node
**Goal:** Set up your environment so you have the `ansible` command available.

**Step 1: Check your OS version**
Open your terminal. If you are on Windows, ensure you are using WSL (Ubuntu).
```bash
cat /etc/os-release
```

**Step 2: Update your package repositories**
It is best practice to update your system before installing new software.
```bash
sudo apt update
```

**Step 3: Install Ansible**
Run the install command. We use `-y` to automatically say "yes" to prompts.
```bash
sudo apt install ansible -y
```

---

### Lab 1.2: Verify Installation
**Goal:** Ensure Ansible was installed correctly and check the version.

**Step 1: check version**
```bash
ansible --version
```

**Expected Output Breakdown:**
- You should see something like `ansible [core 2.12.x]`.
- Look at the `python version` line. It tells you which Python Ansible is using (e.g., 3.8 or 3.10).
- Look at `config file`. It might say `None` or `/etc/ansible/ansible.cfg`. This is where Ansible looks for settings.

---

### Lab 1.3: Creating Your First Inventory & Ping
**Goal:** Tell Ansible about the servers you want to manage. For now, we will manage the machine we are currently on (`localhost`).

**Step 1: Create a workspace folder**
Keep your files organized.
```bash
mkdir ~/ansible-labs
cd ~/ansible-labs
```

**Step 2: Create the `inventory.ini` file**
Ansible needs a list of computers to talk to. We will create a file named `inventory.ini`.
```bash
nano inventory.ini
```
Paste the following content inside:
```ini
[local]
localhost ansible_connection=local
```
*> **Explanation:** We created a group called `[local]`. We added `localhost`. We told Ansible "don't use SSH, just connect locally" with `ansible_connection=local`.*

**Step 3: Test Connectivity (Ping)**
We will use the `ping` module. This is **not** the same as the network `ping` command. It checks:
1. Can I connect?
2. Is Python installed on the target?

Run this command:
```bash
ansible all -i inventory.ini -m ping
```

**Command Breakdown:**
- `all`: Target all hosts in the inventory.
- `-i inventory.ini`: Use the inventory file we just made.
- `-m ping`: Use the `ping` module.

**Expected Output:**
```json
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
If you see **"pong"**, congratulations! You have successfully run your first Ansible command.
