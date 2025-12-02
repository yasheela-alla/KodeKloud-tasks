# Day 10 – Linux Bash Scripts
## Objective

Create `/scripts/ecommerce_backup.sh` on **App Server 3** that:

1. Creates a zip archive of:
   `/var/www/html/ecommerce`
2. Names the archive:
   `xfusioncorp_ecommerce.zip`
3. Saves the archive to:
   `/backup/` on App Server 3
4. Copies the archive to the **Nautilus Backup Server** under:
   `/backup/`
5. Does not prompt for a password during copy.
6. Can be run by the server’s regular user (`banner` on App Server 3).
7. Must not use sudo inside the script.
8. Requires `zip` package installed manually beforehand (outside the script).
   
<img width="1492" height="852" alt="Screenshot 2025-12-02 131402" src="https://github.com/user-attachments/assets/f83d1779-a477-4806-a832-dda9772c2e10" />


# Step 1: Log into App Server 3

From the jump host:

```
ssh banner@stapp03.stratos.xfusioncorp.com
```

(Use the credentials provided in the “Details of all Servers” panel.)


# Step 2: Install the zip package

This must be done manually before running the script.

```
sudo yum install -y zip
```

or

```
sudo apt-get install -y zip
```

(depends on OS)

<img width="776" height="821" alt="Screenshot 2025-12-02 131629" src="https://github.com/user-attachments/assets/23ec5998-b381-41e1-ab7b-a8115c9ca44d" />


# Step 3: Create required directories if missing

```
sudo mkdir -p /scripts
sudo mkdir -p /backup
```

Make banner the owner so that the script can run without sudo:

```
sudo chown -R banner:banner /scripts
sudo chown -R banner:banner /backup
```

# Step 4: Enable password-less authentication to the Backup Server

You must configure SSH key-based authentication from App Server 3 to the Backup Server.
This avoids password prompts during the copy step.

Logged in as `banner` on App Server 3:

Generate key if missing:

```
ssh-keygen -t rsa -b 4096
```

Press Enter for all prompts.

Copy the key to the backup server (username for backup server is usually `clint` unless your lab shows otherwise):

```
ssh-copy-id clint@stbkp01.stratos.xfusioncorp.com
```
<img width="1352" height="435" alt="Screenshot 2025-12-02 132029" src="https://github.com/user-attachments/assets/a7b387c3-c91c-4aa6-8f43-5ac26bd361df" />


Test the connection:

```
ssh clint@stbkp01.stratos.xfusioncorp.com
```

It must connect without asking for a password.

# Step 5: Create the backup script `/scripts/ecommerce_backup.sh`

Create the file:

```
nano /scripts/ecommerce_backup.sh
```

Insert the following script:

```
#!/bin/bash

# Variables
SOURCE_DIR="/var/www/html/ecommerce"
LOCAL_BACKUP_DIR="/backup"
REMOTE_BACKUP_DIR="/backup"
ARCHIVE_NAME="xfusioncorp_ecommerce.zip"
REMOTE_USER="clint"
REMOTE_HOST="stbackup.stratos.xfusioncorp.com"

# Create zip archive locally
zip -r "${LOCAL_BACKUP_DIR}/${ARCHIVE_NAME}" "${SOURCE_DIR}"

# Copy archive to Nautilus Backup Server
scp "${LOCAL_BACKUP_DIR}/${ARCHIVE_NAME}" "${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_BACKUP_DIR}/"
```

Notes:

* No sudo commands used inside the script.
* Script uses variables, making it maintainable.
* Using zip -r to include the full directory tree.

Save and exit.


# Step 6: Make script executable

```
chmod +x /scripts/ecommerce_backup.sh
```

<img width="686" height="179" alt="Screenshot 2025-12-02 133110" src="https://github.com/user-attachments/assets/92713fe4-7b93-42e7-8bd9-500db10e3708" />

# Step 7: Test the script

Run it as the normal user:

```
/scripts/ecommerce_backup.sh
```


Expected behavior:

1. A zip file is created at:
   `/backup/xfusioncorp_ecommerce.zip`
2. The file is copied to the Backup Server’s `/backup/`
3. No password prompts appear.

Verify locally:

```
ls -l /backup
```

Verify on backup server:

```
ssh clint@stbackup.stratos.xfusioncorp.com
ls -l /backup
```

You should see `xfusioncorp_ecommerce.zip`.

# Step 8: Task complete

You have met all requirements:

* Script under `/scripts`
* Zip archive creation
* Local backup to `/backup`
* Password-less copy to Backup Server
* No sudo inside the script
* Runs as the regular server user


<img width="995" height="638" alt="image" src="https://github.com/user-attachments/assets/ea17371b-de77-44bc-9b90-5c90dcc32db8" />
