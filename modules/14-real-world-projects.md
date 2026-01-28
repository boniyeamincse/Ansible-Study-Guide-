# Module 14: Real-world Project Labs

## 📌 Project 1: LAMP Stack Deployment
**Goal:** Deploy Linux, Apache, MySQL, PHP.

**Structure:**
- **Roles:** `common`, `apache`, `mysql`, `php`.
- **Inventory:** `prod`.
- **Playbook:** `site.yml`.

**Tasks:**
1. **Common:** Install vim, git, update cache.
2. **Apache:** Install, push vhost config.
3. **MySQL:** Install server, create DB, create User.
4. **PHP:** Install php-fpm, php-mysql.

## 📌 Project 2: Multi-Environment Deployment

**Goal:** Same playbook, different variables for Dev vs Prod.

**Setup:**
- `inventory/dev` -> `group_vars/all.yml` (debug: true)
- `inventory/prod` -> `group_vars/all.yml` (debug: false)

**Command:**
```bash
ansible-playbook -i inventory/dev site.yml
ansible-playbook -i inventory/prod site.yml
```

## 📌 Project 3: CI/CD Integration
**Goal:** Run Ansible from Jenkins/GitHub Actions.

**GitHub Actions Example:**
```yaml
name: Ansible Deploy
on: push
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run Playbook
        run: ansible-playbook -i inventory site.yml
```
