# Nautilus Project – Day 4 Task

## Overview
The sysadmin team at **xFusionCorp Industries** has developed a new automation script named `xfusioncorp.sh` to handle backup processes.  
Although the script has been distributed to all required servers, it currently lacks the correct permissions on **App Server 2** within the **Stratos Datacenter**.  

Our task is to grant **executable permissions** to the script so that all users can execute it.


## Infrastructure Details

| Server Name | IP             | Hostname                          | User   | Password | Purpose        |
|-------------|----------------|-----------------------------------|--------|----------|----------------|
| stapp02     | 172.16.238.11  | stapp02.stratos.xfusioncorp.com   | steve  | Am3ric@  | Nautilus App 2 |


## Requirements
- Locate the script at:
```

/tmp/xfusioncorp.sh

````
- Grant **executable permissions** to the script.  
- Ensure **all users** have execution rights.


## Steps to Implement

### 1. Connect to App Server 2
From the jump host, SSH into **App Server 2**:
```bash
ssh steve@stapp02.stratos.xfusioncorp.com
````

Password:

```
Am3ric@
```


### 2. Verify Current Permissions

Check the current permissions on the script:

```bash
ls -l /tmp/xfusioncorp.sh
```

Expected output (before change, example):

```
-rw-r--r-- 1 root root 1234 Aug 23 04:21 /tmp/xfusioncorp.sh
```

Here, there is no **executable (`x`)** permission.


### 3. Grant Executable Permissions

Use `chmod` to make the script executable by **all users**:

```bash
sudo chmod a+x /tmp/xfusioncorp.sh
```


### 4. Verify Updated Permissions

Re-check permissions:

```bash
ls -l /tmp/xfusioncorp.sh
```

Expected output:

```
-rwxr-xr-x 1 root root 1234 Aug 23 04:21 /tmp/xfusioncorp.sh
```
<img width="1628" height="762" alt="Screenshot 2025-09-28 161724" src="https://github.com/user-attachments/assets/dea6d9e1-7cd8-464d-889a-8ee6c8abf00c" />

---

This confirms that **all users** can now execute the script.

---

<img width="936" height="559" alt="Screenshot 2025-09-28 161828" src="https://github.com/user-attachments/assets/ea0d0460-d281-40e5-97a3-1f78b29791c4" />
