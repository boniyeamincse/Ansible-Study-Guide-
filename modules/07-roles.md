# Module 7: Roles & Ansible Galaxy

## 📌 Topics
- **Roles:** The standard way to package Ansible automation. Separation of concerns.
- **Directory Structure:**
  - `roles/myrole/tasks/main.yml`
  - `roles/myrole/vars/main.yml`
  - `roles/myrole/templates/`
- **Ansible Galaxy:** A hub for sharing roles (`ansible-galaxy` command).

---

## 🧪 Labs

### Lab 7.1: Create a Custom Nginx Role
**Goal:** Refactor playbook into a role.

**Commands:**
```bash
mkdir -p roles/nginx_role/{tasks,handlers}
```

**File:** `roles/nginx_role/tasks/main.yml`
```yaml
---
- name: Install Nginx
  apt:
    name: nginx
    state: present

- name: Start Nginx
  service:
    name: nginx
    state: started
```

**Use it in `site.yml`:**
```yaml
---
- hosts: webservers
  become: yes
  roles:
    - nginx_role
```

### Lab 7.2: Use Galaxy Roles
**Goal:** Download a community role.

**Command:**
```bash
ansible-galaxy install geerlingguy.mysql
```
