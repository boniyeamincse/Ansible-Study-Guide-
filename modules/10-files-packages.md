# Module 10: File & Package Management

## 📌 Topics
- **File Management:** `copy`, `template`, `file` (perms, symlinks, delete).
- **Package Management:** `package` (generic), `apt` (Debian), `yum`/`dnf` (RHEL), `pip` (Python).

---

## 🧪 Labs

### Lab 10.1: Managing Directories & Symlinks
**Goal:** Create structure and cleanup.

```yaml
  tasks:
    - name: Create Log Dir
      file:
        path: /var/log/myapp
        state: directory
        mode: '0755'

    - name: Create Symlink
      file:
        src: /var/log/myapp
        dest: /home/user/logs
        state: link
```

### Lab 10.2: Install Specific Versions
**Goal:** Install a specific version of a package.

```yaml
    - name: Install Python Requests
      pip:
        name: requests
        version: 2.25.1
```
