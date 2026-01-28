# Module 8: Handlers & Notifications

## 📌 Topics
- **Handlers:** Special tasks that only run when notified.
- **Notifications:** The `notify: <Handler Name>` directive.
- **Use Case:** Restarts services only if configuration files were changed.

---

## 🧪 Labs

### Lab 8.1: Restart Apache on Config Change
**Goal:** Create a playbook that updates config and restarts only if needed.

**File:** `playbooks/handler_demo.yml`
```yaml
---
- name: Handler Demo
  hosts: all
  become: yes

  tasks:
    - name: Update Configuration
      copy:
        content: "ServerName localhost"
        dest: /etc/apache2/conf-available/servername.conf
      notify: Restart Apache

  handlers:
    - name: Restart Apache
      service:
        name: apache2
        state: restarted
```
**Verification:**
1. Run playbook -> Changed=True, Handler Runs.
2. Run again -> Changed=False, Handler **Skipped**.
