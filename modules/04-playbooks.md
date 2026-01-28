# Module 4: Ansible Playbooks

## 📌 Topics
- **What is a Playbook?** A YAML file describing a set of tasks to run on hosts.
- **Structure:**
  - `hosts`: Target machines.
  - `vars`: Variables for the play.
  - `tasks`: The list of actions.
- **Idempotency:** The concept that running a task multiple times should not change the result if the state is already correct.
- **Become:** Privilege escalation (sudo).

---

## 🧪 Labs

### Lab 4.1: Install Apache
**Goal:** Write a playbook to install `apache2` (Ubuntu) or `httpd` (CentOS).

**File:** `playbooks/install_apache.yml`
```yaml
---
- name: Install Apache Web Server
  hosts: all
  become: yes

  tasks:
    - name: Install Apache (Ubuntu)
      apt:
        name: apache2
        state: present
      when: ansible_os_family == "Debian"

    - name: Install Apache (RedHat)
      yum:
        name: httpd
        state: present
      when: ansible_os_family == "RedHat"
```

**Run:**
```bash
ansible-playbook playbooks/install_apache.yml
```

### Lab 4.2: Deploy Website & Services
**Goal:** Create a default index page and ensure the service runs.

**File:** `playbooks/deploy_site.yml`
```yaml
---
- name: Deploy Web Site
  hosts: all
  become: yes

  tasks:
    - name: Copy Index Page
      copy:
        content: "<h1>Ansible is Cool!</h1>"
        dest: /var/www/html/index.html

    - name: Ensure Apache is Started
      service:
        name: "{{ 'apache2' if ansible_os_family == 'Debian' else 'httpd' }}"
        state: started
        enabled: yes
```

### Lab 4.3: Debugging
**Goal:** Print messages during run.

```yaml
    - name: Print Info
      debug:
        msg: "The OS is {{ ansible_distribution }}"
```
