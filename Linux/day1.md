# Custom Apache User Setup on xFusionCorp App Server

This document provides a clear, step-by-step explanation of how to create a custom Apache user on **App Server 2** within the Stratos Datacenter. This task is part of the **Linux Practice Labs on KodeKloud** and aligns with the security standards adopted by **xFusionCorp Industries**.

---

## Overview

To strengthen application security, the security team at xFusionCorp Industries requires dedicated system users for specific Apache-based applications. Each application runs under its own custom user, isolating processes and minimizing security risks.

In this task, we are required to:

- Create a user named **`ravi`**
- Assign a custom UID: **1503**
- Set the home directory to **`/var/www/ravi`**
- Perform all operations on **App Server 2 (stapp02)**

Server authentication must be performed through the **Jump Host** provided in the Stratos Datacenter.

---

## Infrastructure Information

Below are the relevant login details for this task:

| Server       | Hostname                        | User  | Password   | Purpose            |
| ------------ | -------------------------------- | ----- | ---------- | ------------------ |
| App Server 2 | `stapp02.stratos.xfusioncorp.com` | steve | Am3ric@    | Nautilus App 2     |
| Jump Host    | `jump_host.stratos.xfusioncorp.com` | thor  | mjolnir123 | Access gateway     |

> **Note:** All SSH access to application servers must be routed through the **Jump Host**.

---

## Step-by-Step Implementation

### 1. Connect to the Jump Host

Use SSH to connect to the entry point of the Stratos Datacenter:

```bash
ssh thor@jump_host
````

Enter the corresponding password when prompted.

---

### 2. Connect to App Server 2

Once logged into the jump host, connect to the target application server:

```bash
ssh steve@stapp02
```

Enter the provided password to complete the connection.

---

### 3. Create the Custom Apache User

Run the following command to create user `ravi` with the specified UID and home directory:

```bash
sudo useradd -u 1503 -d /var/www/ravi -m ravi
```

**Explanation of options:**

* `-u 1503`: assigns the custom UID
* `-d /var/www/ravi`: sets the designated home directory
* `-m`: ensures that the home directory is created automatically

---

### 4. Verify User Creation

Verify that the user and home directory were created correctly.

Check user details:

```bash
id ravi
```

Check passwd file entry:

```bash
grep ravi /etc/passwd
```

Verify that the home directory exists:

```bash
ls -ld /var/www/ravi
```

You should see:

* UID is **1503**
* Home directory exists at **/var/www/ravi**
* Ownership is assigned to user **ravi**

---

<img width="991" height="652" alt="image" src="https://github.com/user-attachments/assets/00439025-e580-4114-a912-be1cd8fd9e44" />


## Conclusion

This task establishes a secure and isolated environment for Apache applications by creating a dedicated system user with a custom UID and home directory. Following these standards ensures improved security and maintainability across the xFusionCorp application stack.

