# Module 9: Templates & Jinja2

## 📌 Topics
- **Jinja2:** templating language for Python/Ansible.
- **Templates:** Files ending in `.j2` that contain variables/logic.
- **Use:** Creating dynamic config files (e.g., Nginx config with correct domain names).

---

## 🧪 Labs

### Lab 9.1: Nginx Configuration Template
**Goal:** Create a dynamic HTML file.

**File:** `templates/index.html.j2`
```html
<h1>Welcome to {{ ansible_hostname }}</h1>
<p>This server has {{ ansible_memtotal_mb }} MB of RAM.</p>
<ul>
{% for item in my_list %}
  <li>{{ item }}</li>
{% endfor %}
</ul>
```

### Lab 9.2: Deploy Template
**File:** `playbooks/deploy_template.yml`
```yaml
---
- name: Deploy Dynamic Page
  hosts: all
  become: yes
  vars:
    my_list:
      - Apple
      - Banana

  tasks:
    - name: Push Template
      template:
        src: templates/index.html.j2
        dest: /var/www/html/index.html
```
