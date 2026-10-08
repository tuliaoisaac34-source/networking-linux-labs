# Linux Administration ⚙️

Welcome to my Linux Administration repository! This section marks **Week 2** of my portfolio journey. It focuses on the fundamental concepts of identity access control, permissions, process auditing, package management, and basic service orchestration.

---

## 👤 1. Identity & Group Management
Linux is inherently a multi-user operating system. Access control relies on isolating user identities and grouping them by roles.

* **useradd** – Creates a new system user account (e.g., `sudo useradd -m username` provisions a home directory).
* **usermod** – Modifies system account attributes (e.g., `sudo usermod -aG groupname username` appends a user to a supplementary group).
* **userdel** – Deletes a user account from the system (`userdel -r` removes their entire home folder).
* **groupadd** – Provisions a new security group to manage shared permissions for multiple users.

---

## 🛡️ 2. File Permissions & Ownership
Every single file and directory on Linux belongs to an **Owner** and a **Group**, with permissions categorized into three action types: Read (`r`), Write (`w`), and Execute (`x`).

* **chown** – Changes file or folder ownership (e.g., `sudo chown root:sysops file.txt`).
* **chmod** – Changes file or folder access permissions.
  * **Symbolic method:** `chmod +x script.sh` (adds execution rights).
  * **Numeric/Octal method:** `chmod 750 folder/` (Owner = Full access `7`, Group = Read/Execute `5`, Others = No access `0`).

---

## ⚙️ 3. Process Monitoring & Service Management
Administrators must ensure required background applications (daemons) are running smoothly and killing resource-heavy tasks when necessary.

* **ps aux** – Generates a static snapshot of every active running process on the system.
* **top** / **htop** – Launches an interactive, real-time resource monitor to watch CPU and RAM utilization.
* **systemctl** – Controls `systemd` background services:
  * `sudo systemctl status nginx` – Inspects if a application service is running or crashed.
  * `sudo systemctl start nginx` – Manually fires up a stopped service.
  * `sudo systemctl stop nginx` – Gracefully terminates a running service process.

---

## 📦 4. Package Management (APT / DNF)
Software installation, dependencies, and OS patches are safely handled by native package distribution tools.

* **sudo apt update** – Syncs your system's local repository index with online software mirrors.
* **sudo apt upgrade** – Downloads and installs available software package updates.
* **sudo apt install <package>** – Fetches and configures a specific utility from the official repository.

---

## 🧪 5. Week 2 Mini Lab: User and Directory Access

### Objective
Demonstrate the **Principle of Least Privilege** by creating a brand-new system user, assigning them to a specialized group, and giving that group isolated read/write rights to a restricted directory while locking out all other unprivileged users.

### Execution Blueprint
```bash
# 1. Build a new security group for operations
sudo groupadd sysops

# 2. Add a new user with a home directory and set a password
sudo useradd -m labuser
sudo passwd labuser

# 3. Append the new user to the sysops group
sudo usermod -aG sysops labuser

# 4. Provision a target secure directory
sudo mkdir -p /opt/secure-data

# 5. Lock down ownership to root and the sysops group
sudo chown root:sysops /opt/secure-data

# 6. Set octal permissions (Owner: rwx, Group: rwx, Others: None)
sudo chmod 770 /opt/secure-data
```

### Verification & Access Testing
To confirm the access controls apply perfectly, switch user execution context:
```bash
# Switch execution context into 'labuser'
su - labuser

# Verify group membership contains 'sysops'
groups

# Navigate to the protected workspace and attempt to create a file
cd /opt/secure-data
touch access_test.txt
ls -l
```

### 📸 Lab Evidence
* **Screenshot 1 — Account & Group Creation:** *(Add image showing your useradd, groupadd, and passwd terminal entries)*
* **Screenshot 2 — Permissions Check:** *(Add image showing the output of `ls -ld /opt/secure-data` to prove it shows `drwxrwx--- root sysops`)*
* **Screenshot 3 — Successful Access Test:** *(Add image showing that `labuser` successfully generated `access_test.txt` inside the folder without a permission error)*

### 🧠 What I Learned
* **The Importance of Others (`---`):** Dropping others' octal value to `0` locks out any user not explicitly listed as the owner or a group member, protecting internal application configurations.
* **Context Switching with `su`:** Switching users directly inside the terminal allows administrators to simulate precisely how permissions affect regular accounts without logging out of the server environment.

