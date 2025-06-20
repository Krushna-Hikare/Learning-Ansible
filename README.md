# Learning-Ansible
This project runs a basic ansible playbook.

# 🔧 Ansible Nginx Installation on EC2

This project sets up Nginx on two EC2 instances (`dbserver` and `developerserver`) using an Ansible playbook from a third EC2 instance (`ansible` control node).

---

## 📌 Steps to Setup

### 1. Launch 3 EC2 Instances
- `ansible` instance — control node (Ubuntu)
- `dbserver` instance — target node
- `developerserver` instance — target node

---

### 2. Setup SSH Access
On the **ansible** instance:
```bash
ssh-keygen
```

Copy the public key to the **dbserver** and **developerserver**:
```
In ansible instance, move to .ssh folder.
copy id_rsa.pub

In dbserver and developerserver, move to .ssh folder.
paste the key into their `authorized_keys` file.
```

---

### 3. Create Ansible Inventory File
On the **ansible** instance:
```bash
mkdir ansible
cd ansible
```

Create a file named `inventory` and add:
```
[dbserver]
//private-ip of 2nd instance
// like 17.**.***.***

[devserver]
//private-ip of 3rd instance
```
---

### 4. Create Ansible Playbook
Create a file `first-playbook.yml` with the following content:

```yaml
---
- name: Install nginx and start it
  hosts: all
  become: true

  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start nginx
      service:
        name: nginx
        state: started
```

---

### 5. Run the Playbook
```bash
ansible-playbook -i inventory first-playbook.yml
```

---

### 6. Verify on db and dev servers
SSH into each server and run:
```bash
sudo systemctl status nginx
```

You should see that Nginx is **active and running**.

---

## 🧪 Verbose Modes in Ansible

- `-v` : Basic info
- `-vv` : More details (e.g., task-level info)
- `-vvv` : Debug-level detail
- `-vvvv` : Maximum detail including connection info

Example:
```bash
ansible-playbook -vvv -i inventory first-playbook.yml 
```

---

## ✅ Done!

You have successfully configured Nginx on remote EC2s using Ansible 🎉
