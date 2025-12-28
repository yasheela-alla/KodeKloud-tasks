# Creating a Non-Interactive System User on App Server1

This document explains how to create a system user with a non-interactive shell on App Server 1 in the Stratos Datacenter. This configuration is required to meet the specifications of the backup agent tool used by xFusionCorp Industries.


## Objective

The backup agent operates as a background service and does not require interactive shell access. To comply with security best practices, the task requires:

* Creating a user named `kirsty`
* Assigning a **non-interactive shell**
* Performing the operation on **App Server 1 (stapp01)**

<img width="649" height="403" alt="image" src="https://github.com/user-attachments/assets/5de49996-1386-4a27-836d-72ab30a37760" />


## Infrastructure Information

| Server       | Hostname                          | User | Password   | Purpose           |
| ------------ | --------------------------------- | ---- | ---------- | ----------------- |
| App Server 1 | stapp01.stratos.xfusioncorp.com   | tony | Ir0nM@n    | Nautilus App 1    |
| Jump Host    | jump_host.stratos.xfusioncorp.com | thor | mjolnir123 | Datacenter access |

All access to the application servers is routed through the jump host.

---

## Step-by-Step Implementation

### 1. Connect to the Jump Host

```bash
ssh thor@jump_host
```

Authenticate using the provided password.


### 2. Connect to App Server 1

```bash
ssh tony@stapp01
```

Enter the password to complete the login.



### 3. Create User with a Non-Interactive Shell

Create the user `kirsty` using `/sbin/nologin` as the shell:

```bash
sudo useradd -s /sbin/nologin kirsty
```

Explanation:

* `-s /sbin/nologin` ensures the user cannot initiate interactive login sessions
* The user can still be used by system services such as backup agents


### 4. Verify User Configuration

Confirm the user entry and assigned shell:

```bash
grep kirsty /etc/passwd
```

Expected output should show:

```
kirsty:x:UID:GID::/home/kirsty:/sbin/nologin
```

You can also validate using:

```bash
getent passwd kirsty
```


<img width="754" height="591" alt="image" src="https://github.com/user-attachments/assets/56e1ce36-6a07-4074-80bd-32a60042282e" />


## Conclusion

This task ensures that service accounts used by automated tools, such as backup agents, follow the principle of least privilege by disabling interactive shell access. Implementing non-interactive users improves system security while maintaining required functionality.

This approach aligns with xFusionCorp Industries’ operational and security standards across the Stratos Datacenter.
