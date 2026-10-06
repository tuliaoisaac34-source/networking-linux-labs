# Linux CLI Fundamentals & System Administration**NDG Linux Unhatched Reference & System Hardening Documentation**
---## 🖥️ System Architecture & Interface Paradigms

┌────────────────────────────────────────────────────────┐
│ User / Shell │
├────────────────────────────────────────────────────────┤
│ CLI (Bash/Sh) │ GUI (X11/Wayland) │
├──────────────────────────┴─────────────────────────────┤
│ Linux Kernel │
├────────────────────────────────────────────────────────┤
│ Hardware │
└────────────────────────────────────────────────────────┘


*   **CLI (Command-Line Interface):** A deterministic, text-based input mechanism communicating directly with the OS kernel via a shell wrapper. Essential for automation scripts, secure remote access, and lower compute/memory resource consumption.
*   **GUI (Graphical User Interface):** A visual abstraction layer mapping window systems (X11/Wayland) onto terminal spaces. Unnecessary for headless enterprise server deployments.
*   **Case-Sensitivity:** The underlying Linux virtual filesystem layers (ext4, XFS) treat character cases distinctly at the byte level.
    *   `ls Documents` executes successfully.
    *   `ls documents` fails with `No such file or directory` if the lowercase target folder does not exist.
*   **Open Source Governance:** Distributed under the **GNU General Public License (GPL)**, ensuring codebase audatability, open redistribution architectures, and deep infrastructure flexibility.

### 🧩 Command Syntax Mechanics
Linux shell parsers evaluate terminal strings according to a strict three-part hierarchical sequence:

$$\text{Command} \longrightarrow \text{Options/Flags} \longrightarrow \text{Arguments}$$

1.  **Command:** The primary executable binary or built-in shell tool triggered (e.g., `ls`).
2.  **Options/Flags:** Switched modifiers prefixed with a hyphen (`-`) to alter processing behaviors (e.g., `-l`, `-a`). Multiple flags can be grouped into an efficient singular block (e.g., `-la`).
3.  **Arguments:** The target entities or path elements upon which the command acts (e.g., `/var/log`).

---

## 🛠️ Detailed CLI Command Matrix

### 1. Filesystem Directory Listing (`ls`)
Exposes operational metadata records bound to object directories.

*   `ls` — Outputs a simple layout grid of visible folder contents.
*   `ls -l` — Invokes long-listing format, breaking down systemic security and storage allocations:
    ```text
    -rw-r--r--  1  sysadmin  sysadmin  4096  Oct 06 09:26  production.log
    ▲└───┬───┘  ▲      ▲         ▲       ▲        ▲               ▲
    │    │      │      │         │       │        │               └─ Filename
    │    │      │      │         │       │        └─ Modification Timestamp
    │    │      │      │         │       └─ File Size in Bytes
    │    │      │      │         └─ Group Owner
    │    │      │      └─ User Owner
    │    │      └─ Hard Link Count
    │    └─ Permissions Triad (User, Group, Other)
    └─ File Type Indicator ( - = Regular File, d = Directory, l = Symbolic Link )
    ```
*   `ls -a` — Exposes all items including hidden dotfiles prefixed with a leading period (`.`), such as configuration scripts (`.bashrc`).
*   `ls -t` — Orders outputs chronologically based on file modification timestamps.
*   `ls -S` — Orders outputs by storage capacity size footprints.
*   `ls -r` — Reverses the current sorting hierarchy (e.g., `ls -laSr` lists smallest to largest hidden entries).

### 2. Navigation & Path Traversal (`pwd`, `cd`)
Provides mechanics for shifting contextual execution tracks across the filesystem tree.

*   `pwd` — *Print Working Directory*. Evaluates and outputs the absolute shell environment coordinate tracking string from the system root.
*   `cd` — *Change Directory*. Re-maps active working coordinate sets.
    *   **Absolute Paths:** Paths evaluated directly from the system root (`/`) down to a target child node, invariant of current location (e.g., `cd /var/log/nginx`).
    *   **Relative Paths:** Paths evaluated relative to the active working terminal coordinate context (e.g., if inside `/var`, running `cd log/nginx`).
*   **Navigation Shortcuts:**
    *   `..` — Traverses exactly one hierarchy level backward into the parent directory.
    *   `.` — References the immediate working directory context explicitly.
    *   `~` — Resolves dynamically to the logged-in user's system home directory path (`/home/$USER`).
    *   `-` — Toggles back to the previous working directory context.

### 3. File Operations & Stream Analysis

#### Content Inspection
*   `cat` — *Concatenate*. Streams complete raw data sequences from specified text files directly to standard output (`stdout`). Best reserved for short configuration profiles.
*   `head -n [X]` — Limits standard output to the exact first `X` lines of a file stream (defaults to 10 if `-n` is omitted).
*   `tail -n [X]` — Limits standard output to the final `X` lines of a file stream.
    *   `tail -f` — *Follow mode*. Keeps the file stream open dynamically to print incoming lines in real-time. Essential for live log troubleshooting.
*   `less` — Interactive terminal pager utility. Allows backward and forward scrolling navigation through large log structures without loading the entire asset block into system memory.

#### Data Manipulation & Block Copying
*   `touch` — Instantly creates an empty file if the target does not exist, or updates the access and modification timestamps of an existing file.
*   `cp` — Copies file arrays across target folder routes.
    *   `cp -r` — Recursively copies directories, maintaining structural hierarchies.
    *   *Usage Example:* `cp /etc/passwd .` (Copies system credential metrics directly into the active working context dot directory).
*   `mv` — Moves files across filesystem endpoints. Also performs atomic inline renames when destination targets remain within localized directories.
*   `rm` — Permanently purges standard files from the filesystem index.
    *   `rm -r` — Recursively deletes directories and all nested children data structures.
    *   `rm -f` — Overrides interactive confirmations, forcing immediate elimination. **Warning:** Irreversible in standard environments.
*   `dd` — Low-level, block-by-block bitstream duplicator. Used for bare-metal backups, partition cloning, and master boot record isolation.
    *   *Syntax Parameters:* `if=` (Input File/Device), `of=` (Output File/Device Target), `bs=` (Block Size execution speed override), `count=` (Total blocks to process).
    *   *Production Command:* `sudo dd if=/dev/sda of=/backup/disk_image.raw bs=4M`

---

## 🔐 Administrative Privilege & Access Control

### 🛂 Identity Switching Mechanics
Linux maintains strict isolation between standard user profiles and the administrative root operating layer.

*   `su` — *Switch User*. Switches the active shell context to an alternate profile. Requires target user's password.
*   `su -` (or `su -l` / `su --login`) — Invokes a complete login shell transformation. Purges current environmental variables and instantiates the target profile's explicit profile paths, environment variables, and shell configurations.
*   `sudo` — *Superuser Do*. Executes a single target operation utilizing the elevated privilege scopes of the root environment based on configurations inside the `/etc/sudoers` safety policy layout. Requires the *current* user's password, reducing shared credential risks.
*   `exit` — Terminates the active sub-shell instance, dropping the session back down to the preceding profile prompt level.

### 📄 Permission Matrix Layout & Modification
File attributes map directly to a strict access matrix split among three functional entities: **User (u)**, **Group (g)**, and **Other (o)**.

#### Octal vs. Symbolic Permission Architecture
Permissions translate between alphabetic characters and binary weight positions:

| Permission | Character | Binary Value | Octal Weight |
| :--- | :--- | :--- | :--- |
| **Read** | `r` | `100` | **4** |
| **Write** | `w` | `010` | **2** |
| **Execute** | `x` | `001` | **1** |

```text
Octal Calculation Matrix Example:
  r w x  │  r - x  │  r - -
  4+2+1  │  4+0+1  │  4+0+0
   (7)   │   (5)   │   (4)   --> Resulting Mode: 754
```

#### Modifying Permissions (`chmod`, `chown`)
*   `chmod` — *Change Mode*. Adjusts security flags on assets.
    *   **Symbolic Assignment:** `chmod g+w,o-r security_profile.txt` (Adds write permission to Group, strips read from Others).
    *   **Octal Assignment:** `chmod 755 deployment.sh` ( Grants `rwx` to Owner, `r-x` to Group, and `r-x` to Others).
*   `chown` — *Change Owner*. Assigns alternative user and group operational ownership layers.
    *   *Production Command:* `sudo chown root:sysadmin infrastructure.conf`

---

## 🔍 Text Filtering & Stream Manipulation (`grep`)

The `grep` (*Global Regular Expression Print*) engine filters data streams to locate and output explicit patterns parsed out of standard input or plain-text files.

*   *Usage Example:* `grep 'sysadmin' /etc/passwd` (Extracts target identity parameters from system user records).
*   `grep -i` — Skips case distinction constraints entirely during scanning sweeps.
*   `grep -v` — Inverts the filter, returning only lines that **do not** match the pattern.
*   `grep -E` — Activates Extended Regular Expression features (equivalent to using the legacy `egrep` command).

### 🧩 Regular Expression (RegEx) Reference Matrix

| Token | Engine Mode | Functional Behavior Mode | Syntax Example |
| :--- | :--- | :--- | :--- |
| `.` | Basic (`grep`) | Matches any single character placeholder. | `d.g` matches *dog*, *dig* |
| `[ ]` | Basic (`grep`) | Validates a single match out of a strict bracket array. | `[A-Z]` matches any uppercase character |
| `[^ ]` | Basic (`grep`) | Inverts check; matches characters outside bracket boundaries. | `[^0-9]` matches any non-digit character |
| `*` | Basic (`grep`) | Evaluates zero or more consecutive recurrences of the preceding token. | `ab*` matches *a*, *ab*, *abbb* |
| `^` | Basic (`grep`) | Anchors pattern execution strictly to the start of a text stream line. | `^root` matches lines starting with *root* |
| `$` | Basic (`grep`) | Anchors pattern execution tracking directly to the end of a line. | `false$` matches lines ending with *false* |
| `+` | Extended (`grep -E`) | Evaluates one or more occurrences of the preceding token pattern. | `sh+` matches *sh*, *shh*, but not *s* |

| ? | Extended (grep -E) | Marks the preceding element string token as optional. | logs? matches log or logs |
| \| | Extended (grep -E) | Executes alternate comparison tests mirroring a logical OR block. | aws\|gcp matches aws or gcp |
------------------------------
## ⚡ System Administration & Diagnostics## 🛑 System Lifecycle & Foreground Control

* shutdown now — Signals immediate execution of systemic level-0 operational shifts, safely halting services and cutting core mainboard power states.
* shutdown +1 "Emergency Maintenance Inbound" — Broadcasts system-wide notification warning banners directly to active shell terminals before enforcing a 1-minute delayed drop cycle.
* Ctrl + C — Broadcasts an explicit SIGINT (Signal Interrupt) tracking vector directly to active foreground processes, terminating run states and restoring immediate shell input prompt fields.
* date — Standard output format showing current system calendar, clock tracking runtime parameters, and timezone metrics.

## 🌐 Network Diagnostics & Interface Topology

* ifconfig — Outputs detailed structural mapping parameters covering legacy network interfaces, IP bindings, and MAC hardware validation IDs.
* iwconfig — Dedicated wireless interface utility exposing link operational qualities, SSID tracking metrics, and radio operational configurations.
* ping -c 4 1.1.1.1 — Dispatches exactly 4 network diagnostic ICMP Echo Request frames to verify absolute round-trip network connectivity vectors to a target host.

## ⚙️ Process Audits & Resource Tracking (ps)
Exposes detailed operational allocation records mapping active kernel processes back to tracking IDs.

* Process Table Structure Metadata Fields:
* PID: Process Identifier. The discrete numerical tracking address assignment key used by the kernel.
   * TTY: TeleTypewriter. Identifies the explicit control terminal channel managing the operation.
   * TIME: Total cumulative execution processor utilization clock block counts.
   * CMD: The specific command binary instantiation call string that triggered the process.
* ps — Standard display loop showing active tasks running inside the user's current terminal instance.
* ps -e — Pulls data strings mapping every active runtime execution thread throughout the system namespace.
* ps -ef — Forces full details layout parsing (-f) across all active background processing steps to expose parent-child process chains and parameter flags.



