# Module 12: Ansible Vault & Security

## 📌 Topics
- **Ansible Vault:** Encrypting files or strings using AES-256.
- **Commands:** `create`, `edit`, `encrypt`, `decrypt`, `view`.
- **Usage:** Passwords, API keys.

---

## 🧪 Labs

### Lab 12.1: Create Encrypted File
**Steps:**
1. Run:
   ```bash
   ansible-vault create secrets.yml
   ```
2. Enter password.
3. Add vars: `db_pass: 12345`

### Lab 12.2: Run Playbook with Vault
**Steps:**
```bash
ansible-playbook site.yml --ask-vault-pass
```

### Lab 12.3: Encrypt String (Inline)
**Goal:** Encrypt just one variable value.
```bash
ansible-vault encrypt_string 'MySecretPassword' --name 'db_password'
```
*Copy output to your playbook.*
