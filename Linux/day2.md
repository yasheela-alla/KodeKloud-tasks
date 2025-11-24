# Group-Based Access Control Setup on Stratos Datacenter App Servers

This document outlines the steps required to implement group-based access control across the application servers in the Stratos Datacenter. The task reflects the access management standards followed by the system administration team at xFusionCorp Industries.

<img width="1654" height="900" alt="image" src="https://github.com/user-attachments/assets/2c79bd4c-2e23-4758-9ea8-8f3d9610f0de" />


## Objective

To standardize access permissions across all application servers, the following actions must be performed on **App Server 1**, **App Server 2**, and **App Server 3**:

1. Create a group named `nautilus_noc`.
2. Ensure that a user named `stark` exists.
3. Add the user `stark` to the `nautilus_noc` group.

These steps must be carried out on each app server individually.


## Infrastructure Information

| Server Name | Hostname                          | User   | Password   | Purpose        |
| ----------- | --------------------------------- | ------ | ---------- | -------------- |
| stapp01     | stapp01.stratos.xfusioncorp.com   | tony   | Ir0nM@n    | Nautilus App 1 |
| stapp02     | stapp02.stratos.xfusioncorp.com   | steve  | Am3ric@    | Nautilus App 2 |
| stapp03     | stapp03.stratos.xfusioncorp.com   | banner | BigGr33n   | Nautilus App 3 |
| jump_host   | jump_host.stratos.xfusioncorp.com | thor   | mjolnir123 | Access gateway |

All application servers must be accessed through the jump host.


## Step-by-Step Implementation

### 1. Connect to the Jump Host

Start by authenticating to the jump host:

```bash
ssh thor@jump_host
```

Enter the provided password when prompted.


### 2. Connect to Each App Server

Once on the jump host, connect to each app server sequentially:

```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

Use the corresponding passwords for each connection.


## 3. Create the Group on Each App Server

Run the following command on each server:

```bash
sudo groupadd nautilus_noc
```

If the group already exists, the system will notify you, and no further action is needed for this step.


## 4. Ensure User stark Exists

Check if the user exists:

```bash
id stark
```

If the user does not exist, create it:

```bash
sudo useradd stark
```

## 5. Add User stark to the Group

Add the user to the group using:

```bash
sudo usermod -aG nautilus_noc stark
```

Verify the group membership:

```bash
id stark
```

You should see `nautilus_noc` listed in the user’s secondary groups.

## Verification

Perform the verification steps on each app server:

* Confirm that the group exists:

```bash
getent group nautilus_noc
```

* Confirm that the user belongs to the correct group:

```bash
id stark
```

Proper output should list `nautilus_noc` as one of the user’s groups.

## Conclusion

This task establishes consistent group-based access management across all application servers in the Stratos Datacenter. Creating the `nautilus_noc` group and ensuring user assignment enhances security, simplifies access control, and aligns with xFusionCorp's operational standards.

<img width="979" height="596" alt="image" src="https://github.com/user-attachments/assets/7488d0b0-a343-492c-8b36-a6bd139acf1a" />
