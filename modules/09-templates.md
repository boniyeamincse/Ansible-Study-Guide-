# Module 9: Templates & Jinja2

## 📌 Topics
- **Jinja2:** The Python templating language Ansible uses.
- **Templates (.j2):** Files that contain variables (e.g., `{{ port_number }}`) and logic (e.g., `{% if ... %}`).
- **Module:** `template` module replaces variables with actual values and copies the file to the remote host.

---

## 🧪 Labs

### Lab 9.1: Creating a Dynamic HTML Page
**Goal:** Deploy a website `index.html` that shows specific information about the server it is running on.

**Step 1: Create the Template file**
Create a folder `templates` and the file inside it.
```bash
mkdir templates
nano templates/index.html.j2
```

**Content:**
```html
<html>
<head><title>Ansible Managed</title></head>
<body>
    <h1>Welcome to {{ ansible_hostname }}</h1>
    <p>This server is running: {{ ansible_distribution }} {{ ansible_distribution_version }}</p>
    <p>Memory Available: {{ ansible_memtotal_mb }} MB</p>

    <h3>Installed Packages Loop:</h3>
    <ul>
    {% for item in my_packages %}
        <li>{{ item }}</li>
    {% endfor %}
    </ul>
</body>
</html>
```

**Step 2: Create the Playbook**
Create `playbooks/deploy_template.yml`.
```yaml
---
- name: Deploy Dynamic Website
  hosts: all
  become: yes
  vars:
    my_packages:
      - git
      - curl
      - nginx

  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Deploy Index Template
      template:
        src: ../templates/index.html.j2
        dest: /var/www/html/index.html
      notify: Restart Nginx

  handlers:
    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```

**Step 3: Run and Verify**
```bash
ansible-playbook playbooks/deploy_template.yml
```
Curl the local server to see the result:
```bash
curl localhost
```
*You should see the variable names replaced with actual server values.*

---

### Lab 9.2: Logic in Configuration Files
**Goal:** Configure Nginx differently depending on a variable `listen_port`.

**Step 1: Create Config Template**
`templates/nginx.conf.j2`:
```nginx
server {
    listen {{ listen_port }};
    server_name {{ ansible_fqdn }};
}
```

**Step 2: Use in Playbook with a variable**
```yaml
    - name: Push Config
      template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/sites-enabled/default
      vars:
        listen_port: 8080
```
This allows you to change the port for hundreds of servers just by changing one variable.
