# Nautilus Project – Day 2 Task

## Overview
As part of the temporary assignment to the **Nautilus Project**, a developer named **kirsty** requires access to **App Server 1** for a limited duration.  
To ensure proper access management and automatic expiry, we will create a temporary user account with a defined expiry date.


## Requirements
- Create a user named **kirsty** (all lowercase, as per standard protocol).
- User must be created on **App Server 1** in the **Stratos Datacenter**.
- Set the account expiry date to **2024-01-28**.
- Verify that the expiry date has been correctly applied.


## Steps to Implement

### 1. Access the Jump Host
SSH into the jump host using provided credentials:
```bash
ssh thor@jump_host.stratos.xfusioncorp.com
````

Password:

```
mjolnir123
```

### 2. Connect to App Server 1

From the jump host, connect to **App Server 1**:

```bash
ssh tony@stapp01
```

> *Replace `stapp01` with the correct hostname if different (check the details section of the environment).*

### 3. Create the User with Expiry Date

Run the following command to create user **kirsty** with expiry date **2024-01-28**:

```bash
sudo useradd -e 2024-01-28 kirsty
```

* `-e YYYY-MM-DD` → sets the account expiry date.

### 4. Verify the User’s Expiry Date

To confirm the expiry date:

```bash
sudo chage -l kirsty | grep "Account expires"
```

Expected output:

```
Account expires : Jan 28, 2024
```

## Example Output

```bash
$ sudo useradd -e 2024-01-28 kirsty
$ sudo chage -l kirsty | grep "Account expires"
Account expires : Jan 28, 2024
```


## Notes

* After **2024-01-28**, user **kirsty** will no longer be able to log in, ensuring temporary access is properly revoked.
* To remove the expiry (if needed later), run:

  ```bash
  sudo chage -E -1 kirsty
  ```
* Always follow the **lowercase username standard** for consistency.


## Verification Checklist

✅ User `kirsty` exists on App Server 1
✅ Expiry date is set to `2024-01-28`
✅ Account is temporary and will auto-disable on expiry

```
```

<img width="1735" height="876" alt="Screenshot 2025-09-24 191825" src="https://github.com/user-attachments/assets/c7677961-e0c8-4730-be8d-ae5554912de8" />

----

<img width="1724" height="981" alt="Screenshot 2025-09-24 193503" src="https://github.com/user-attachments/assets/ee9b0c51-c871-4766-8a9a-d08c7f0ca29a" />
