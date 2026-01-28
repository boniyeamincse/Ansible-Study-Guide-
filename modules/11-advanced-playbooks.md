# Module 11: Advanced Playbooks

## 📌 Topics
- **Imports vs Includes:**
  - `import_tasks`: Static. Parsed before run. Good for basic file splitting.
  - `include_tasks`: Dynamic. Parsed during run. Good for loops.
- **Strategies:**
  - `linear`: Default. All hosts finish Task 1 before moving to Task 2.
  - `free`: Hosts race to the finish line independently.
- **Error Handling:**
  - `block / rescue / always`: Like Try/Catch/Finally in programming.

---

## 🧪 Labs

### Lab 11.1: Error Handling with Block/Rescue
**Goal:** Simulate a failed upgrade and handle the error gracefully by "rolling back".

**Step 1: Create `playbooks/rollback_demo.yml`**
```yaml
---
- name: Upgrade System
  hosts: all
  become: yes

  tasks:
    - block:
        - name: Attempt Critical Upgrade
          command: /bin/false  # Intentionally failing command
          
        - name: Restart Service
          service:
            name: myservice
            state: restarted

      rescue:
        - name: Rollback / Recovery Message
          debug:
            msg: "Upgrade failed! Rolling back changes..."

      always:
        - name: Cleanup
          debug:
            msg: "This runs whether the upgrade failed or succeeded."
```

**Step 2: Run it**
```bash
ansible-playbook playbooks/rollback_demo.yml
```
*Observation:* The play does **not** crash. Instead, it enters the `rescue` section, prints the rollback message, then runs `always`, and finishes successfully (green).

---

### Lab 11.2: Playbook Strategies
**Goal:** Speed up execution for independent hosts using `strategy: free`.

**Step 1: Create `playbooks/fast_deploy.yml`**
```yaml
---
- name: Fast Deployment
  hosts: all
  strategy: free  # Creating the race condition
  tasks:
    - name: Sleep for 5 seconds
      command: sleep 5
      
    - name: Sleep for 2 seconds
      command: sleep 2
```
If you had 100 hosts, the fast ones wouldn't wait for the slow ones at every step.
