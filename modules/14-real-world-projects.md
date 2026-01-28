# Module 14: Real-world Project Labs

## 📌 Scenario
You are a DevOps Engineer. You need to automate the deployment of a LAMP stack (Linux, Apache, MySQL, PHP) capable of hosting a dynamic website.

---

## 🚀 Project 1: The LAMP Stack Playbook

**Goal:** Create a complete automation pipeline.

**Files to create:**
- `inventory/prod.ini`
- `roles/common/` (basic tools like git, vim)
- `roles/web/` (apache, php)
- `roles/db/` (mysql)
- `site.yml` (Master playbook)

### Step 1: Inventory
`inventory/prod.ini`:
```ini
[web]
web1.example.com

[db]
db1.example.com
```

### Step 2: Site Playbook
`site.yml`:
```yaml
---
- name: Apply Common Config
  hosts: all
  become: yes
  roles:
    - common

- name: Setup Database
  hosts: db
  become: yes
  roles:
    - db

- name: Setup Webserver
  hosts: web
  become: yes
  roles:
    - web
```

---

## 🚀 Project 2: Multi-Environment Deployment

**Goal:** Use the **same code** for Dev and Prod, but with different configurations.

**Step 1: Directory Structure**
```text
group_vars/
  all.yml      # Global settings
inventory/
  dev/
    hosts.ini
    group_vars/
      all.yml  # Dev specific (debug: true)
  prod/
    hosts.ini
    group_vars/
      all.yml  # Prod specific (debug: false)
```

**Step 2: Execution**
Deploy to Dev:
```bash
ansible-playbook -i inventory/dev/hosts.ini site.yml
```
Deploy to Prod:
```bash
ansible-playbook -i inventory/prod/hosts.ini site.yml
```

---

## 🚀 Project 3: CI/CD Pipeline (GitHub Actions)

**Goal:** Run Ansible automatically when you push code to GitHub.

**Step 1: Create Workflow File**
`.github/workflows/deploy.yml`:
```yaml
name: Ansible Deploy
on: [push]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Run Playbook
        uses: dawidd6/action-ansible-playbook@v2
        with:
          playbook: site.yml
          directory: ./
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          inventory: ${{ secrets.INVENTORY }}
```
*> **Note:** You must add `SSH_PRIVATE_KEY` to your GitHub Repository Secrets.*
