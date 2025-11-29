# Nautilus Project – Cron Automation Task
The Nautilus system administrators team is preparing to deploy automated scripts across all application servers in the **Stratos Datacenter**.  
Before deploying production jobs, they want to test cron functionality using a simple sample job.

This document outlines the required steps to install cron services and configure a scheduled task on **all Nautilus App Servers**.

<img width="643" height="501" alt="Screenshot 2025-11-29 175426" src="https://github.com/user-attachments/assets/ce173ae5-1903-47fa-bc86-de16ea243168" />

## Target Systems

| Server Name | Hostname                        | User  | Purpose          |
|-------------|----------------------------------|--------|------------------|
| stapp01     | stapp01.stratos.xfusioncorp.com | tony   | Nautilus App 1   |
| stapp02     | stapp02.stratos.xfusioncorp.com | steve  | Nautilus App 2   |
| stapp03     | stapp03.stratos.xfusioncorp.com | banner | Nautilus App 3   |

_All steps must be performed on **each** of the three app servers._


## Requirements

### **a. Install `cronie` package and start `crond` service**  
Cron may not be installed or running by default.  
You must:

1. Install the `cronie` package  
2. Enable and start the `crond` service


### **b. Add the following cron job for the root user:**

```

*/5 * * * * echo hello > /tmp/cron_text

````

This job executes **every 5 minutes** and writes `hello` into `/tmp/cron_text`.


## Steps to Implement

### 1. SSH into each App Server
Example:

```bash
ssh tony@stapp01.stratos.xfusioncorp.com
````

Repeat using the appropriate user for stapp02 and stapp03.


### 2. Install the Cronie Package

Run:

```bash
sudo yum install -y cronie
```

Or for Debian-based systems (if applicable):

```bash
sudo apt-get update
sudo apt-get install -y cron
```

### 3. Start and Enable the Cron Service

```bash
sudo systemctl start crond
sudo systemctl enable crond
```

Verify service status:

```bash
systemctl status crond
```
<img width="1696" height="927" alt="image" src="https://github.com/user-attachments/assets/4bd67488-6402-4577-b070-6aaa8bea2532" />


### 4. Add the Cron Job for Root

Open the root user's crontab:

```bash
sudo crontab -e
```

Add the line:

```
*/5 * * * * echo hello > /tmp/cron_text
```

Save and exit.


### 5. Verify the Cron Job Is Installed

Check root's cron list:

```bash
sudo crontab -l
```

Expected output:

```
*/5 * * * * echo hello > /tmp/cron_text
```
<img width="962" height="198" alt="image" src="https://github.com/user-attachments/assets/ab204bbc-ecd6-4e39-b6b9-64c99d7ec8d1" />

## Verification Checklist

* [x] `cronie` package installed on **stapp01**, **stapp02**, **stapp03**
* [x] `crond` service started and enabled on all servers
* [x] Root user has the cron job:

  ```
  */5 * * * * echo hello > /tmp/cron_text
  ```
* [x] `/tmp/cron_text` is created and updated every 5 minutes

---

<img width="1095" height="725" alt="image" src="https://github.com/user-attachments/assets/9a9f5be7-f292-42a0-8c3f-78e97dc13890" />


