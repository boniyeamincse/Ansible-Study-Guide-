# Module 12: Ansible Vault & Security

## 📌 Topics
- **Ansible Vault:** Encrypting files or strings using AES-256.
- **Commands:** `create`, `edit`, `encrypt`, `decrypt`, `view`.
- **Best Practice:** Keep passwords in `vars/secrets.yml` and encrypt that file.

---

## 🧪 Labs

### Lab 12.1: Encrypting a Sensitive File
**Goal:** Create a file containing a database password and encrypt it so it can be committed to Git safely.

**Step 1: Create and Encrypt**
```bash
ansible-vault create secrets.yml
```
Ansible will ask for a Vault Password (e.g., `ansible123`).
Then it opens your default text editor (vim/nano).

**Step 2: Add content**
```yaml
db_password: "SuperSecretPassword!"
api_key: "XYZ-123-456"
```
Save and close.

**Step 3: View the file**
Try to `cat` it.
```bash
cat secrets.yml
```
You will see garbage (AES encrypted text).
To view it properly:
```bash
ansible-vault view secrets.yml
```

---

### Lab 12.2: Using Encrypted Vars in a Playbook
**Goal:** Use the secret password in a task without exposing it in cleartext yaml.

**Step 1: Create `playbooks/vault_demo.yml`**
```yaml
---
- name: Security Demo
  hosts: localhost
  connection: local
  
  # Load the encrypted file
  vars_files:
    - ../secrets.yml

  tasks:
    - name: Debug Secret (Don't do this in production logs!)
      debug:
        msg: "The password is {{ db_password }}"
```

**Step 2: Run with Vault Password**
Ansible needs the password to decrypt the file in memory.
```bash
ansible-playbook playbooks/vault_demo.yml --ask-vault-pass
```
*Enter `ansible123`. You should see the decrypted password in the output.*

---

### Lab 12.3: Encrypting Single Strings (Encrypt String)
**Goal:** Encrypt just one line inside a regular YAML file.
```bash
ansible-vault encrypt_string 'MySecretP@ss' --name 'my_pass'
```
Copy the output block and paste it into any variables file.
