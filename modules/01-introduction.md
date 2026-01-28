# Module 1: Introduction to Ansible

## 📌 Topics

### 1.1 What is Ansible?
Ansible is an open-source automation tool suitable for:
- **Configuration Management**: Ensuring all servers have the same configuration (e.g., typical web server settings).
- **Application Deployment**: Deploying your apps from Git to your servers.
- **Orchestration**: Managing complex workflows involving many different servers.

It was created by Michael DeHaan in 2012 and acquired by Red Hat in 2015. It is written in **Python**.

### 1.2 Why do we require Ansible?
In the past, system administrators managed servers manually or with shell scripts.
- **Manual approach:** Slow, error-prone, hard to scale.
- **Shell scripts:** Hard to maintain, not always idempotent (running them twice might break things).
- **Ansible:** Provides a readable, version-controlled, and idempotent way to manage thousands of servers as easily as one.

### 1.3 Ansible Architecture
Ansible uses a very simple architecture:

1.  **Control Node:** The machine where you run the `ansible` command. It can be your laptop (Mac/Linux/WSL) or a central server (Tower/Jenkins). *Note: The Control Node cannot be Windows.*
2.  **Managed Nodes:** The target servers (Linux, Windows, Network Devices) you want to configure.
3.  **Inventory:** A list of managed nodes (IP addresses or Hostnames).
4.  **Modules:** Small pieces of code (written in Python/PowerShell) that Ansible pushes to the nodes to perform tasks (e.g., `apt`, `copy`, `service`).
5.  **Plugins:** Extend Ansible's core functionality (e.g., logging, connection types).

### 1.4 How Ansible Works?
1.  **Read Input:** It reads your Playbook (YAML) and Inventory.
2.  **Connect:** It connects to the Managed Nodes using **SSH** (secure shell) or WinRM (for Windows).
3.  **Push Modules:** It copies the necessary modules (small Python scripts) to the temporary directory on the managed nodes.
4.  **Execute:** It runs those modules on the remote nodes.
5.  **Clean up:** It removes the modules and reports the result ("Changed", "OK", "Failed") back to the Control Node.

### 1.5 Benefits of Using Ansible
1.  **Agentless:** No software to install on the target servers. If you can SSH into it, you can manage it.
2.  **Simple (YAML):** Playbooks are written in human-readable YAML, not complex code.
3.  **Idempotent:** You can run the same playbook 100 times, and it will only make changes if the system is not in the desired state.
4.  **Powerful:** Can manage not just Linux, but Windows, Networking gear (Cisco, Juniper), and Cloud (AWS, Azure).

### 1.6 Push-Based vs Pull-Based Approach
Ansible uses a **Push-Based** model by default.

| Feature | Push Based (Ansible) | Pull Based (Puppet/Chef) |
| :--- | :--- | :--- |
| **How it starts** | You run command on Control Node -> Pushes to servers. | Agent on Server wakes up -> Asks master for updates. |
| **Agent** | No Agent (SSH). | Requires Agent on every server. |
| **Control** | Full control over **when** updates happen. | Updates happen when the agent checks in (e.g., every 30 mins). |
| **Setup** | Fast setup (Seconds). | Slower setup (Install agents, certificates). |

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
