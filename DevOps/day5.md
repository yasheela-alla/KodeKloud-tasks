# Nautilus Project – Day 5 Task

## Overview
Following a security audit, the **xFusionCorp Industries** security team has decided to strengthen application and server security using **SELinux**.  
As part of the initial rollout, the following requirements have been established for **App Server 3** in the **Stratos Datacenter**:

1. Install the required SELinux packages.  
2. Permanently **disable SELinux** for now (it will be re-enabled after configuration changes).  
3. No need to reboot the server immediately (a maintenance reboot is scheduled).  
4. Ignore the current runtime SELinux status — the important part is that SELinux will remain **disabled after reboot**.  


## Infrastructure Details

| Server Name | IP             | Hostname                          | User   | Password | Purpose        |
|-------------|----------------|-----------------------------------|--------|----------|----------------|
| stapp03     | 172.16.238.12  | stapp03.stratos.xfusioncorp.com   | banner | BigGr33n | Nautilus App 3 |


## Steps to Implement

### 1. Connect to App Server 3
SSH into the server from the jump host:
```bash
ssh banner@stapp03.stratos.xfusioncorp.com
````

Password:

```
BigGr33n
```

### 2. Install SELinux Packages

Ensure SELinux and its required tools are installed:

```bash
sudo yum install -y selinux-policy selinux-policy-targeted policycoreutils
```

> If using a Debian/Ubuntu-based system:

```bash
sudo apt-get update
sudo apt-get install -y selinux selinux-utils policycoreutils
```

<img width="1686" height="909" alt="image" src="https://github.com/user-attachments/assets/916ef489-fe1c-49d6-a176-43c7cffe660b" />


### 3. Permanently Disable SELinux

Edit the SELinux configuration file:

```bash
sudo vi /etc/selinux/config
```

Find the line:

```
SELINUX=enforcing
```

Change it to:

```
SELINUX=disabled
```

<img width="702" height="206" alt="image" src="https://github.com/user-attachments/assets/efdf4c09-caab-46bb-898c-fe6fb2ab3f61" />


Save and exit.


### 4. Verify Configuration File

Check the file to ensure SELinux is disabled:

```bash
grep SELINUX= /etc/selinux/config
```

Expected output:

```
SELINUX=disabled
```

<img width="845" height="315" alt="image" src="https://github.com/user-attachments/assets/320633ff-e953-4af2-99ea-d962f3aa7587" />


## Notes

* **Do not reboot now** – a scheduled reboot will apply the changes tonight.
* After the reboot, SELinux will be **permanently disabled**.
* Current runtime status (from `getenforce` or `sestatus`) may still show `Enforcing` or `Permissive` until the reboot occurs.

---

## Verification Checklist

✅ SELinux packages are installed on **stapp03**

✅ `/etc/selinux/config` is updated with `SELINUX=disabled`

✅ No reboot was performed (scheduled reboot will enforce the change)

✅ After reboot, SELinux will remain disabled permanently

```
```
<img width="926" height="545" alt="image" src="https://github.com/user-attachments/assets/928951b1-db7c-4afb-9f32-612c427d024c" />
