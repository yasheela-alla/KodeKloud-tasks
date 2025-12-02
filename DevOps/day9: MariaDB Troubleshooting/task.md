
# MariaDB / MySQL Troubleshooting Playbook

### A reusable guide for restoring database services in DevOps and SRE workflows
<img width="1592" height="488" alt="Screenshot 2025-12-02 123025" src="https://github.com/user-attachments/assets/e00f059c-9cd7-4e81-8478-20cf620fe723" />

## 1. Confirm the service name

MariaDB and MySQL use different unit names depending on OS family:

```bash
systemctl status mariadb
systemctl status mysql
```
<img width="1685" height="773" alt="Screenshot 2025-12-02 123226" src="https://github.com/user-attachments/assets/ab6cd4ab-c48d-447b-b300-2aa120fe035b" />

If neither exists, the service may not be installed.

---

## 2. Attempt to start the service

```bash
sudo systemctl start mariadb
```

Check status:

```bash
sudo systemctl status mariadb
```

If the service starts normally, the issue is resolved.

---

## 3. If the service fails, check journal logs

```bash
sudo journalctl -xeu mariadb
```
<img width="810" height="947" alt="Screenshot 2025-12-02 123443" src="https://github.com/user-attachments/assets/6f53ff92-b0ca-43a7-abcd-12f04acabda8" />

Look for keywords:

* `permission denied`
* `file not found`
* `corrupted`
* `socket` errors
* `port already in use`
* `can't find service`
* `InnoDB` errors

This identifies the category of failure.

---

## 4. Category-Based Fixes

### A. Missing package (most common in labs)

If the unit file does not exist:

```
Unit mariadb.service not found.
```

Install the server:

```bash
sudo yum install -y mariadb-server
```
<img width="1343" height="957" alt="Screenshot 2025-12-02 123807" src="https://github.com/user-attachments/assets/d711f87b-97b6-4605-8238-e5f169817b34" />

or on Debian/Ubuntu:

```bash
sudo apt-get install -y mariadb-server
```

Start and enable:

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```
<img width="1701" height="922" alt="Screenshot 2025-12-02 123850" src="https://github.com/user-attachments/assets/1f5f1f1a-68d2-4e3d-8f28-202547172b03" />

---

### B. Data directory missing or corrupted

Common error messages:

* `Cannot open datafile`
* `ibdata1 is missing`
* `InnoDB: Unable to find tablespace`

Restore or recreate the database directory:

```bash
sudo mysql_install_db --user=mysql
sudo systemctl start mariadb
```

If `/var/lib/mysql` is corrupted beyond repair in a lab:
You can remove and reinitialize it:

```bash
sudo rm -rf /var/lib/mysql
sudo mysql_install_db --user=mysql
sudo systemctl start mariadb
```

### C. Permission issues

Errors include:

* `Permission denied`
* `Can't create/write to file mysql.sock`
* `Access denied to /var/lib/mysql/`

Fix ownership:

```bash
sudo chown -R mysql:mysql /var/lib/mysql
sudo chmod -R 755 /var/lib/mysql
sudo systemctl start mariadb
```

### D. Port 3306 is already in use

Check:

```bash
sudo lsof -i :3306
```

Terminate the conflicting process:

```bash
sudo kill -9 <PID>
sudo systemctl start mariadb
```

### E. Broken or incomplete configuration file

If `/etc/my.cnf` or `/etc/mysql/my.cnf` contains invalid configuration, the server won’t start.

Inspect and revert recent changes:

```bash
sudo vi /etc/my.cnf
```

Comment suspicious lines and try again:

```bash
sudo systemctl start mariadb
```

## 5. Validate database functionality

### A. Check socket

```bash
ls -l /var/lib/mysql/mysql.sock
```

### B. Login as root

```bash
mysql -u root -p
```

### C. Confirm listening port

```bash
sudo ss -tulpn | grep 3306
```

## 6. Ensure service starts automatically

```bash
sudo systemctl enable mariadb
```

## 7. Final practicality notes

1. In real production, always take a backup of `/var/lib/mysql` before deleting or reinitializing.
2. In labs like KodeKloud, destructive resets are fine because the environment is isolated.
3. Many automated graders accept **package install + start + enable** as the correct fix.
4. If unsure:

   * Always check the service status.
   * Always check the logs.
   * Always fix the category of error, not random commands.
     
<img width="1012" height="649" alt="Screenshot 2025-12-02 124149" src="https://github.com/user-attachments/assets/1b5c37fc-1818-4e68-834e-5e565ce6e7a4" />


# Quick-Reference Summary

```bash
systemctl status mariadb
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo journalctl -xeu mariadb

sudo yum install -y mariadb-server
sudo mysql_install_db --user=mysql
sudo chown -R mysql:mysql /var/lib/mysql
sudo lsof -i :3306
```
