# Module 8: Handlers & Notifications

## 📌 Topics
- **Handlers:** Special tasks that only run when notified. They are typically used to restart services.
- **Notifications:** The `notify: <Handler Name>` directive.
- **Behavior:**
  - Handlers run at the very end of the play.
  - Handlers only run if the notifying task **changed** something.

---

## 🧪 Labs

### Lab 8.1: Restart Apache Only on Config Change
**Goal:** Create a playbook that updates a configuration file. If the file changes, restart Apache. If not, do nothing.

**Step 1: Create `playbooks/handler_demo.yml`**
```yaml
---
- name: Handler Demo
  hosts: all
  become: yes

  tasks:
    - name: install apache
      package:
        name: apache2
        state: present

    - name: Update Configuration
      copy:
        content: "ServerName localhost"
        dest: /etc/apache2/conf-available/servername.conf
      notify: Restart Apache  # This must match Handler name exactly

    - name: Enable Config (Apache specific command)
      command: a2enconf servername
      args:
        creates: /etc/apache2/conf-enabled/servername.conf

  handlers:
    - name: Restart Apache
      service:
        name: apache2
        state: restarted
```

**Step 2: Run execution #1**
```bash
ansible-playbook playbooks/handler_demo.yml
```
*Observation:* The "Update Configuration" task shows `changed`. At the end of the play, "Restart Apache" runs.

**Step 3: Run execution #2**
```bash
ansible-playbook playbooks/handler_demo.yml
```
*Observation:* "Update Configuration" is green (no change). The handler is **SKIPPED** (it doesn't even show up). This is efficient automation!
