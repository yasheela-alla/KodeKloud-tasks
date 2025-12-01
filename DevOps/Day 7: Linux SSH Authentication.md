# 🚀 Setting Up Password-less SSH Access from Jump Host to App Servers (Two Methods)

In the Stratos Datacenter, **xFusionCorp Industries** relies on automation scripts executed from the **Jump Host**.
These scripts must connect to all **Nautilus App Servers** without requiring passwords.

To achieve this, we configure **password-less SSH access** from:

* `thor@jump-host` → `tony@stapp01`
* `thor@jump-host` → `steve@stapp02`
* `thor@jump-host` → `banner@stapp03`

This article demonstrates **two clean and reliable methods**:

1. **Manual Setup (Recommended for Exams)**
2. **Automated Bash Script (Efficient for DevOps Workflows)**

---

# 📌 Server Details

| Server       | User   | Hostname                            |
| ------------ | ------ | ----------------------------------- |
| Jump Host    | thor   | `jump_host.stratos.xfusioncorp.com` |
| App Server 1 | tony   | `stapp01.stratos.xfusioncorp.com`   |
| App Server 2 | steve  | `stapp02.stratos.xfusioncorp.com`   |
| App Server 3 | banner | `stapp03.stratos.xfusioncorp.com`   |

---

# 🟢 METHOD 1 – Manual Password-less SSH Setup (Step-by-Step)

This method is ideal for hands-on labs and KodeKloud Engineer tasks.

---

## **Step 1 — Log into the Jump Host as thor**

```bash
ssh thor@jump_host.stratos.xfusioncorp.com
```

---

## **Step 2 — Generate an SSH keypair (if not already generated)**

```bash
ssh-keygen -t rsa -b 4096
```

Press **ENTER** for all prompts.

Your keys will be stored at:

* `~/.ssh/id_rsa` (private key)
* `~/.ssh/id_rsa.pub` (public key)

---

## **Step 3 — Copy the public key to each app server**

### App Server 1

```bash
ssh-copy-id tony@stapp01.stratos.xfusioncorp.com
```

### App Server 2

```bash
ssh-copy-id steve@stapp02.stratos.xfusioncorp.com
```

### App Server 3

```bash
ssh-copy-id banner@stapp03.stratos.xfusioncorp.com
```

You will enter each password only once.

---

## **Step 4 — Verify password-less access**

```bash
ssh tony@stapp01.stratos.xfusioncorp.com
ssh steve@stapp02.stratos.xfusioncorp.com
ssh banner@stapp03.stratos.xfusioncorp.com
```

If all connect without asking for a password → 🎉 **Success!**

---

# 🟦 METHOD 2 – Automated Script for Password-less SSH Setup

This method is ideal for automating multi-server access in DevOps environments.

---

<img width="1712" height="826" alt="image" src="https://github.com/user-attachments/assets/a04dec7e-515c-4228-afdd-5e84806a7b3e" />


### **script.sh**

```bash
#!/bin/bash

# Declare associative array: server => user
declare -A servers=(
    ["stapp01"]="tony"
    ["stapp02"]="steve"
    ["stapp03"]="banner"
)

# Generate SSH key for thor if it doesn't exist
if [ ! -f "$HOME/.ssh/id_rsa" ]; then
    echo "Generating SSH key for thor..."
    ssh-keygen -t rsa -N "" -f "$HOME/.ssh/id_rsa"
else
    echo "SSH key already exists. Skipping generation."
fi

# Copy public key to each app server
for server in "${!servers[@]}"; do
    user="${servers[$server]}"
    echo "Setting up password-less SSH for $user@$server..."
    ssh-copy-id -o StrictHostKeyChecking=no "$user@$server"
done

echo "✅ Password-less SSH setup completed for all app servers."
```

---

## **How to Run the Script**

Save the script:

```bash
nano script.sh
```

Make it executable:

```bash
chmod +x script.sh
```

Run:

```bash
./script.sh
```

---

## Example Output

```text
Generating SSH key for thor...
Setting up password-less SSH for banner@stapp03...
Setting up password-less SSH for steve@stapp02...
Setting up password-less SSH for tony@stapp01...
✅ Password-less SSH setup completed for all app servers.
```

---

## **Test SSH Access**

```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```
<img width="1002" height="666" alt="image" src="https://github.com/user-attachments/assets/68259366-c03f-43a1-a3fd-48a9829a9c57" />


---

# 🟢 Verification Checklist

| Requirement                             | Status |
| --------------------------------------- | ------ |
| thor → stapp01 as tony (passwordless)   | ✔      |
| thor → stapp02 as steve (passwordless)  | ✔      |
| thor → stapp03 as banner (passwordless) | ✔      |
| SSH keys stored in `~/.ssh`             | ✔      |
| Automation-friendly access established  | ✔      |

---

# 🎉 Final Notes

* **Manual method** → perfect for exam labs / KKE tasks.
* **Script method** → perfect for DevOps teams with multiple servers.
* **Never share your private SSH key (`id_rsa`)**.
* If something breaks, check permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Both methods achieve a secure, scalable, and automation-ready SSH environment.!


---

<img width="973" height="593" alt="image" src="https://github.com/user-attachments/assets/9a0ea214-ddf7-4038-858b-2958651ee7e4" />
