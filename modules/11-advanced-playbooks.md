# Module 11: Advanced Playbooks

## 📌 Topics
- **Imports vs Includes:**
  - `import_playbook`: Static (at parse time).
  - `include_tasks`: Dynamic (at runtime).
- **Strategies:** `linear` (default), `free` (fast as possible).
- **Error Handling:** `block`, `rescue`, `always`.

---

## 🧪 Labs

### Lab 11.1: Error Handling (Block/Rescue)
**Goal:** Attempt upgrade, rollback if failed.

```yaml
---
- hosts: all
  tasks:
    - block:
        - name: Upgrade DB (Simulated)
          command: /bin/false # This will fail
      rescue:
        - name: Rollback
          debug:
            msg: "Original task failed. Rolling back..."
      always:
        - name: Cleanup
          debug:
            msg: "This runs no matter what."
```

### Lab 11.2: Strategies
**Goal:** Run independently.
```yaml
- hosts: all
  strategy: free
  tasks: ...
```
