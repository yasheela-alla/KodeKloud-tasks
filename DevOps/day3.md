# Nautilus Project – Day 3 Task

## Overview
Following recent security audits, the **xFusionCorp Industries** security team has rolled out new protocols.  
One of the critical requirements is to **restrict direct SSH root login** across all application servers within the **Stratos Datacenter**.  

This measure enhances security by ensuring administrators connect using non-root accounts and escalate privileges only when necessary.


## Infrastructure Details

| Server Name | IP             | Hostname                          | User   | Password | Purpose          |
|-------------|----------------|-----------------------------------|--------|----------|------------------|
| stapp01     | 172.16.238.10  | stapp01.stratos.xfusioncorp.com   | tony   | Ir0nM@n  | Nautilus App 1   |
| stapp02     | 172.16.238.11  | stapp02.stratos.xfusioncorp.com   | steve  | Am3ric@  | Nautilus App 2   |
| stapp03     | 172.16.238.12  | stapp03.stratos.xfusioncorp.com   | banner | BigGr33n | Nautilus App 3   |


## Requirements
- Disable **direct root login via SSH** on:
  - `stapp01`
  - `stapp02`
  - `stapp03`
- Ensure non-root users (`tony`, `steve`, `banner`) can still connect and use `sudo` for privilege escalation.


## Steps to Implement

### 1. Connect to Each App Server
From the **jump host**, SSH into each app server using its respective credentials:

```bash
ssh tony@stapp01.stratos.xfusioncorp.com
ssh steve@stapp02.stratos.xfusioncorp.com
ssh banner@stapp03.stratos.xfusioncorp.com
````


### 2. Update the SSH Configuration

Edit the SSH daemon configuration file:

```bash
sudo vi /etc/ssh/sshd_config
```

Locate the line:

```
#PermitRootLogin yes
```

Change it to:

```
PermitRootLogin no
```

> If the line is missing, add it at the end of the file.


### 3. Restart the SSH Service

Apply the configuration changes by restarting the SSH daemon:

```bash
sudo systemctl restart sshd
```


### 4. Verify the Change

Check the effective configuration:

```bash
sudo grep PermitRootLogin /etc/ssh/sshd_config
```

Expected output:

```
PermitRootLogin no
```

Test by attempting a root login (should fail):

```bash
ssh root@stapp01.stratos.xfusioncorp.com
```

Expected result:

```
Permission denied
```

## Notes

* This change strengthens security by ensuring **principle of least privilege**.
* If administrative root access is required, users must log in with their respective accounts (`tony`, `steve`, `banner`) and use:

  ```bash
  sudo -i
  ```

```
```

<img width="1664" height="833" alt="Screenshot 2025-09-25 174949" src="https://github.com/user-attachments/assets/aec76edf-96c8-4239-baed-d2e45846d103" />

---

<img width="408" height="203" alt="Screenshot 2025-09-25 174724" src="https://github.com/user-attachments/assets/14e48f30-869d-431b-a307-73a2a00b9058" />

---

<img width="982" height="698" alt="Screenshot 2025-09-25 175901" src="https://github.com/user-attachments/assets/235ae0e0-25b1-465f-9a61-29d1562d2fdb" />

---

<img width="978" height="650" alt="Screenshot 2025-09-25 180010" src="https://github.com/user-attachments/assets/e8de3d0f-d211-43a3-b5c4-96b6ccc5f339" />

