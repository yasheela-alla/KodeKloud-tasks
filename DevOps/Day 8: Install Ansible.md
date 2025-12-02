# Day 8 – Installing Ansible 4.9.0 on the Jump Host (via pip3)

## 1. Goal

The Nautilus DevOps team has decided to use the **jump host** as the Ansible control node.

<img width="672" height="638" alt="Screenshot 2025-12-02 120820" src="https://github.com/user-attachments/assets/2a3d23d9-7431-4995-9c1d-aaafe5d321b4" />

Requirements:

- Install **Ansible 4.9.0** on the **jump host**.
- Installation must be done using **pip3 only** (no `yum install ansible`, `apt install ansible`, etc.).
- The `ansible` binary must be **available globally**, i.e. any user on the system should be able to run `ansible` commands.

This document walks through:

- A straightforward, exam-friendly installation method (what the KodeKloud checker expects).
- An alternative, slightly more controlled method while still using pip3.

To meet the Day 8 requirement:

1. Ensure `python3` and `pip3` exist.

<img width="781" height="521" alt="Screenshot 2025-12-02 121458" src="https://github.com/user-attachments/assets/85e71e2e-e974-4a47-a861-89c602d1204d" />

2. Install Ansible 4.9.0 globally with:

   ```bash
   sudo pip3 install "ansible==4.9.0"
   ```
<img width="1021" height="386" alt="Screenshot 2025-12-02 121934" src="https://github.com/user-attachments/assets/e3db3eaf-93a5-40b3-b34e-5c22d3eff44f" />

3. Confirm:

   ```bash
   which ansible
   ansible --version
   ```
<img width="997" height="308" alt="Screenshot 2025-12-02 121957" src="https://github.com/user-attachments/assets/b0ace24c-99c0-41f8-90d0-7ed819dcf806" />

4. Optionally verify from another user via `sudo -i`.

---
<img width="980" height="617" alt="Screenshot 2025-12-02 122101" src="https://github.com/user-attachments/assets/dbdf4647-b22c-4819-a32b-70c79f6ac930" />

At that point, the jump host is ready to act as an Ansible control node for the rest of the Stratos Datacenter.
