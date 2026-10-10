Welcome to my Linux Administration repository! This section documents **Week 2** of my IT portfolio journey. It covers administrative access, symbolic and numeric permission modifications, file and group ownership configurations, input filtering, and tracking active system processes.

---

## 👤 1. Administrative Access & User Management
Operating safely as a system administrator requires balancing unprivileged tasks with elevated execution contexts.

* **su** – Allows you to act as a different user temporarily.
* **su -**, **su -l**, or **su --login** – Fully logs in as the root administrative user, generating a fresh login shell environment.
* **exit** – Logs out of the current switched user session and returns you to your previous shell command prompt.
* **sudo** – Executes a single, special task with elevated administrative privileges without changing your permanent user context.
* **passwd** – Sets or updates user account passwords (e.g., `passwd fina_user`).
* **useradd** – Creates a brand new user account on the local system (e.g., `useradd -m fina_user`).
* **groupadd** – Establishes a new group container for security and access management (e.g., `groupadd finance`).
* **usermod** – Modifies an existing user's system attributes (e.g., `usermod -aG finance fina_user` appends a user to a supplementary group).
* **id** – Displays real-time user and group IDs (UID/GID) for a specified account to confirm active memberships.
* **passwd -S sysadmin** – Displays password status information. 

| Metadata Field | Example Value | Description |
| :--- | :--- | :--- |
| **User Name** | `sysadmin` | The target user account being audited. |
| **Password Status** | `P` | Account status indicator (`P` = usable, `L` = locked, `NP` = no password). |
| **Last Change Date** | *Date* | The exact calendar day the account password was last modified. |
| **Minimum Days** | `0` | Minimum number of days required before a password can be changed again. |
| **Maximum Days** | `99999` | Maximum days of validity before the system forces a password change. |
| **Warning Days** | `7` | Days ahead of expiration that the system begins prompting the user to update. |

---

## 🛡️ 2. File Ownership & Permissions

Every file and directory layout displays a strict ownership metadata string when evaluated using long listing commands (`ls -l` or `ls -ld`).

### Access Modification Methods

| Method | Syntax Approach | Core Behavior & Mechanics |
| :--- | :--- | :--- |
| **Symbolic Method** | `chmod u+x hello.sh`<br>`chmod o-r hello.sh` | Modifies one specific set of permissions at a time using target letters, explicit operators, and specific action flags. |
| **Absolute (Numeric) Method** | `chmod 770 /finance_data` | Replaces character flags with a 3-digit octal number shortcut representing the entire permission string configuration all at once. |

### Permissions Structural Breakdown

| Target Scope | Operator | Action Flag | Numeric Weight | Description |
| :--- | :--- | :--- | :--- | :--- |
| **u** (User/Owner) | **+** (Add Access) | **r** (Read) | `4` | View file text or list folder directory contents. |
| **g** (Group Owner) | **-** (Remove Access) | **w** (Write) | `2` | Modify file contents or create/delete files inside a folder. |
| **o** (Others/Public) | **=** (Exact Match) | **x** (Execute) | `1` | Run a script/program binary or cross through a folder path. |
| **a** (All Identities) | *N/A* | *N/A* | `0` | Represents zero access (`---`) when no permissions are assigned. |

* **chown** – Changes file or folder user ownership. It can reassign both user and group parameters simultaneously when combined with a colon identifier (e.g., `chown root:finance /finance_data`).

---

## ⚙️ 3. Process Auditing, Package & Power Controls
Administrators use system utilities to audit processes, manage software packages, control background services, and safely transition machine states.

### Process & Service Control Reference
* **ps -e** – Displays a standard static list of every active running process on the system.
* **ps -ef** – Generates an extended layout displaying more details (such as unique Process Identifiers / PIDs, TTY, running time, and the command that started the process).
* **apt / dnf** – System package managers used to download, update, install, or purge binary applications from remote software repositories (e.g., `apt install ufw -y`).
* **systemctl** – The central framework interface used to control system daemons and background processes.

### System Controller Actions

| Control Parameter | Practical Syntax Examples | Functional Outcome |
| :--- | :--- | :--- |
| **start** | `systemctl start ufw` | Boots the application engine into memory instantly for the active session. |
| **enable** | `systemctl enable ufw` | Configures the system initialization scripts to launch the service automatically at boot. |
| **status** | `systemctl status ufw` | Pulls service logs and prints runtime indicators (`active (running)` or `inactive (dead)`). |

* **shutdown now** – Instantly shuts down the machine from the root command context.
* **shutdown +1 "Goodbye World!"** – Schedules an automated system shutdown in 1 minute and broadcasts a custom warning message to all open terminal users.

---

## 🔍 4. Content Filtering & Regular Expressions (grep)
When files or process layouts are too long, you can pass regular expression patterns to isolate lines matching specific strings.

* **grep 'root' /etc/passwd** – Searches files and outputs lines matching the target word.

| Regex Metacharacter | Functional Syntax Pattern | Targeted Evaluation Rule |
| :--- | :--- | :--- |
| **`^`** | `grep '^root' /etc/passwd` | Restricts the string match to the absolute beginning of a line. |
| **`$`** | `grep 'r$' alpha-first.txt` | Restricts the string match to the absolute end of a line. |
| **`.`** | `grep 'r..f' red.txt` | Acts as a wild-card matching exactly one single character of any type. |
| **`[ ]`** | `grep '[0-9]' profile.txt` | Matches any single character specified inside the literal bracket list. |
| **`[^ ]`** | `grep '[^0-9]' profile.txt` | Inverts the match, isolating lines containing characters *not* in the brackets. |
| **`*`** | `grep 'ab*' text.txt` | Matches zero or more continuous occurrences of the preceding character. |


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

* **Screenshot 1 — Context Elevation**
  * Description: Successfully escalated user privileges from the unprivileged `sysadmin` account to the `root` administrative environment using the `su -` command.
  * <img width="432" height="113" alt="Context Elevation via su command" src="https://github.com/user-attachments/assets/526c7405-3df6-459e-9605-ecbe3ede262e" />

* **Screenshot 2 — Troubleshooting Path Traversal**
  * Description: Encountered a `No such file or directory` error due to a missing trailing plural character in the targeted directory path (`Document` vs. `Documents`).
  * <img width="440" height="158" alt="Path Traversal Error and Troubleshooting" src="https://github.com/user-attachments/assets/2b7aa16c-1c43-4eb2-820e-0856db451514" />

* **Screenshot 3 — Final Verification & Symbolic Permissions Output**
  * Description: Verified successful transfer of ownership to `root` and validated the isolated symbolic permission bits (`-rwxr----`) via `ls -l hello.sh`.
  * <img width="455" height="190" alt="Symbolic Permissions Verification Output" src="https://github.com/user-attachments/assets/c3f42f22-53d0-49dd-b86c-c8e2139e1191" />


### 🧠 What I Learned
* **Symbolic Isolation:** I learned how to use targets like `u` and `o` to selectively update file attributes without risking altering or wiping out the rest of the existing permission string.
* **Metadata Fields:** Long display outputs are crucial for validation; reading the owner and group names sitting directly before the file size and timestamp helps confirm identity changes immediately.
