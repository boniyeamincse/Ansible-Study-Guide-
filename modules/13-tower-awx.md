# Module 13: Ansible Tower / AWX (Optional)

## 📌 Topics
- **AWX/Tower:** The Web UI for Ansible.
- **Features:**
  - **Push button deployment:** Non-technical users can run playbooks.
  - **RBAC:** Control who can run what.
  - **Surveys:** Ask users for input (e.g., "Which version to deploy?") before running.
  - **Scheduling:** Run playbooks every night at 2 AM.

---

## 🧪 Labs

### Lab 13.1: Installation (Docker Based)
*> **Note:** AWX is complex. This assumes you have Docker and Docker Compose installed.*

**Step 1: Clone the Installer**
```bash
git clone https://github.com/ansible/awx-operator.git
```
*(Alternative: Use the older Docker Compose method if Operator is too heavy for your lab VM).*

**Step 2: Accessing the UI**
Once running, open your browser at `http://localhost`. The default user is usually `admin` / `password`.

---

### Lab 13.2: Configuring a Job
**Goal:** Run the "Ping" command from the Web UI.

**Step 1: Create an Inventory**
Go to **Inventories** -> **Add**. Make a "Demo Inventory" containing `localhost`.

**Step 2: Create a Credential**
Go to **Credentials**. Add a "Machine Credential". Paste your SSH Private Key.

**Step 3: Create a Job Template**
Go to **Templates**.
- **Playbook:** Choose a playbook from your Project (setup Git Source first).
- **Inventory:** Demo Inventory.
- **Credential:** Your SSH Key.

**Step 4: Launch**
Click the Rocket 🚀 icon. Watch the output stream in the browser window just like a terminal!
