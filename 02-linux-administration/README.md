Welcome to my Linux Administration repository! This section documents **Week 2** of my IT portfolio journey. It covers administrative access, symbolic permission changes, file ownership, input filtering, and tracking active system processes.

---

## 👤 1. Administrative Access & User Management
Operating safely as a system administrator requires balancing unprivileged tasks with elevated execution contexts.

* **su** – Allows you to act as a different user temporarily.
* **su -**, **su -l**, or **su --login** – Fully logs in as the root administrative user, generating a fresh login shell environment.
* **exit** – Logs out of the current switched user session and returns you to your previous shell command prompt.
* **sudo** – Executes a single, special task with elevated administrative privileges without changing your permanent user context.
* **passwd** – Sets or updates user account passwords.
* **passwd -S sysadmin** – Displays password status information. 
  * *Syntax break down:* User name (`sysadmin`), PW status (`P` = usable, `L` = locked, `NP` = no password), last change date, minimum days before changes (`0`), maximum days before expiry (`99999`), and warning days (`7`).

---

## 🛡️ 2. File Ownership & Permissions (Symbolic Method)
Every file and directory layout displays a strict ownership metadata string when evaluated using long listing commands.

* **chown** – Changes the user owner of a file (e.g., `sudo chown root hello.sh`).
* **chmod** – Modifies modes of access. Using the **Symbolic Method**, you change one set of permissions at a time using targets (**u** = user, **g** = group, **o** = other, **a** = all) and operators (**+** = add, **-** = remove, **=** = specify exact match).
  * Permissions are evaluated across three primary actions: **Read** (`r`), **Write** (`w`), and **Execute** (`x`).
  * Example: `chmod u+x hello.sh` adds execution rights specifically to the user owner.

---

## ⚙️ 3. Process Auditing & Power Controls
Administrators use process utilities to keep tabs on what programs are consuming resources and to safely transition machine states.

* **ps -e** – Displays a standard static list of every active running process on the system.
* **ps -ef** – Generates an extended layout displaying more details (such as unique Process Identifiers / PIDs, TTY, running time, and the command that started the process).
* **shutdown now** – Instantly shuts down the machine from the root command context.
* **shutdown +1 "Goodbye World!"** – Schedules an automated system shutdown in 1 minute and broadcasts a custom warning message to all open terminal users.

---

## 🔍 4. Content Filtering & Regular Expressions (grep)
When files or process layouts are too long, you can pass regular expression patterns to isolate lines matching specific strings.

* **grep 'root' /etc/passwd** – Searches files and outputs lines matching the target word.
* **^** – Restricts the match to the absolute beginning of a line (e.g., `grep '^root' /etc/passwd`).
* **\$** – Restricts the match to the absolute end of a line (e.g., `grep 'r$' alpha-first.txt`).
* **.** – Matches any one single character (e.g., `grep 'r..f' red.txt`).
* **[ ]** – Matches any one specified character from a list (e.g., `grep '[0-9]' profile.txt`).
* **[^ ]** – Matches anything *not* containing the specified character (e.g., `grep '[^0-9]' profile.txt`).
* ***** – Matches zero or more occurrences of the previous character.

---

## 🧪 5. Week 2 Mini Lab: User and Directory Access

### Scenario Goal
*"Create a secure Linux file environment where a specific file's ownership is elevated to root, and permissions are modified symbolically to restrict access."*

### Step-by-Step Lab Execution
```bash
# 1. Elevate user context to the root user environment to make changes
su -

# 2. Navigate to your user workspace directory
cd /home/sysadmin/Documents

# 3. Alter file ownership from sysadmin over to root
sudo chown root hello.sh

# 4. Symbolically add execute permissions for the user owner
chmod u+x hello.sh

# 5. Symbolically strip read permissions away from outside 'others'
chmod o-r hello.sh

# 6. Verify your updated ownership and permission metadata string
ls -l hello.sh
```

### 📸 Lab Evidence
* **Screenshot 1 — Context Elevation:** *(Add an image showing your `su -` command switching to the root terminal profile)*
* **Screenshot 2 — Ownership Alteration:** *(Add an image showing your `chown` command modifying the file owner string successfully)*
* **Screenshot 3 — Symbolic Permissions Output:** *(Add an image showing the output of `ls -l hello.sh` confirming your updated permission attributes)*

* <img width="432" height="113" alt="image" src="https://github.com/user-attachments/assets/526c7405-3df6-459e-9605-ecbe3ede262e" />
<img width="440" height="158" alt="image" src="https://github.com/user-attachments/assets/2b7aa16c-1c43-4eb2-820e-0856db451514" />
<img width="455" height="190" alt="image" src="https://github.com/user-attachments/assets/c3f42f22-53d0-49dd-b86c-c8e2139e1191" />

### 🧠 What I Learned
* **Symbolic Isolation:** I learned how to use targets like `u` and `o` to selectively update file attributes without risking altering or wiping out the rest of the existing permission string.
* **Metadata Fields:** Long display outputs are crucial for validation; reading the owner and group names sitting directly before the file size and timestamp helps confirm identity changes immediately.
