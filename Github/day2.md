# Clone Git Repository on Storage Server

This document describes the steps to clone the `beta.git` repository to the Storage Server within the Stratos Datacenter. This task is part of the KodeKloud Engineer Linux/Git practice labs.

---

## Task Overview

The DevOps team created a new Git repository located at **`/opt/beta.git`**, and the Nautilus application team requires a copy of this repository on the **Storage Server**. Your task is to clone the repository into a specific directory using the correct user account without altering any permissions or files.

<img width="1588" height="740" alt="image" src="https://github.com/user-attachments/assets/39cf3e24-14cf-4add-aef1-e28a3d7746e7" />


### Requirements

1. **Repository to clone:**  
   `/opt/beta.git`

2. **Destination directory:**  
   `/usr/src/kodekloudrepos`

3. **User to perform the task:**  
   `natasha` on the **Storage Server (`ststor01`)**

4. No modifications should be made to repository contents, permissions, or existing directories.

---

## Server Access Information

| Server         | Hostname                  | User    | Password     | Purpose             |
|----------------|---------------------------|---------|--------------|---------------------|
| Jump Host      | `jump_host.stratos.xfusioncorp.com` | thor    | mjolnir123   | Gateway access      |
| Storage Server | `ststor01.stratos.xfusioncorp.com`  | natasha | xFusionCorp   | Clone repository    |

All connections to application/storage servers must be made **via the jump host**.

---

## Step-by-Step Instructions

### 1. SSH into the Jump Host

```bash
ssh thor@jump_host
````

Enter the password when prompted.

---

### 2. SSH into the Storage Server

```bash
ssh natasha@ststor01
```

Enter the password for user `natasha`.

---

### 3. Create the Target Directory

The destination path must exist before cloning:

```bash
mkdir -p /usr/src/kodekloudrepos
cd /usr/src/kodekloudrepos
```

---

### 4. Clone the Git Repository

Run the clone command:

```bash
git clone /opt/beta.git
```

Expected output:

```
Cloning into 'beta'...
warning: You appear to have cloned an empty repository.
done.
```

---

### 5. Verify the Result

Confirm the repository directory exists:

```bash
ls -l /usr/src/kodekloudrepos
```

Example output:

```
drwxr-xr-x 3 natasha natasha 4096 Nov 23 07:27 beta
```

This indicates that the repository **cloned successfully** and the directory ownership is correct.

---

## Completion

<img width="934" height="550" alt="image" src="https://github.com/user-attachments/assets/84fb6323-4bf5-4e9c-bea5-9e2c70fd340e" />


🎉 **Task successfully completed.**
You cloned the Git repository to the correct directory using the appropriate user and without altering any system or repository configurations.

