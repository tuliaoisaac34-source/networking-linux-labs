# Linux Basics 🐧

Welcome to my **Linux Basics** repository! This project serves as a structured collection of my core Linux system administration notes, command-line fundamentals, and practical examples. 

Everything documented here represents fundamental competencies required for managing environments, auditing file systems, and manipulating data via the Command-Line Interface (CLI).

---

## 🧭 1. Navigation & Pathing Basics
Linux is completely **case-sensitive** (e.g., `ls Documents` works, but `ls documents` will throw an error).

* `pwd` - Prints the exact working directory path you are currently standing in.
* `cd` - Changes directories to move around the file system.
* **Absolute Paths:** Starts from the absolute root directory (e.g., `cd /home/sysadmin`).
* **Relative Paths:** Starts from where you are right now.
  * `.` - Refers to the current directory.
  * `..` - Moves up one level to the parent directory.
  * `~` - Shortcut to go straight back to your home directory.

---

## 📂 2. File Listing & Metadata (`ls`)
The `ls` command is used for listing directory contents. Combining it with specific flags reveals deep system metadata:

* `ls -a` - Lists all entries, including **hidden files** (any file starting with a dot `.`).
* `ls -l` - Long listing format showing file types, permissions, sizes, and owners.
* `ls -t` - Sorts the results by timestamp (newest files show first).
* `ls -s` - Sorts the output by file size.
* `ls -r` - Reverses the sorting direction of the output.

### Understanding `ls -l` Output Syntax

When running a long listing, the metadata fields map out exactly like this:

```text
-rw-r--r--     1    sysadmin    sysadmin      647      Dec 20 2017    hello.sh
[File Type]        [Owner]      [Group]    [Size]*     [Timestamp]   [Filename]
[Permissions]
```

*\*Note: The raw file size in bytes always sits directly before the modification date and time timestamp.*

---

## 🛡️ 3. Administrative Access & Account Security
* `su` - Switches user context to temporarily act as a different user.
* `su -`, `su -l`, or `su --login` - Fully logs in as the root administrative user.
* `sudo` - Executes a single, special task with elevated administrative privileges.
* `exit` - Logs out of the current session or terminal window.
* `passwd` - Updates or changes user passwords.
* `passwd -S sysadmin` - Audits password status information.
  * Outputs columns tracking: Username ➡️ Status (`P` for usable, `L` for locked, `NP` for none) ➡️ Last change date ➡️ Minimum days before change (`0`) ➡️ Maximum days before expiration (`99999`) ➡️ Expiry warning days (`7`).

---

## 🔐 4. File Permissions & Ownership (`chmod` / `chown`)
Every file has access flags broken into three categories: Read (`r`), Write (`w`), and Execute (`x`).
* File types are denoted at the start: `-` for regular files, `d` for directories, and `l` for symbolic links.
* `chmod` - Changes file access modes using the **Symbolic Method**:
  * Targets: `u` (user), `g` (group), `o` (other), `a` (all).
  * Actions: `+` (add), `-` (remove), `=` (set exact).
  * Example execution: Changing permissions on a script to test it locally: `./hello.sh`
* `chown` - Changes target ownership (e.g., `sudo chown root hello.sh` swaps ownership from sysadmin to root).

---

## 📄 5. File Operations & Data Streams

### Viewing & Modifying Files
* `cat` - Concatenates and quickly displays full contents of small files (e.g., `cd ~/Documents` then `cat animals.txt`).
* `head -n 5 animal.txt` - Displays only the first 5 lines from the top of the file (defaults to 10 lines if `-n` is omitted).
* `tail` - Displays a select number of lines from the bottom of a file.

### Copying, Moving, & Deleting
* `cp /etc/passwd .` - Copies a file from a source path to a destination (using `.` here copies it straight to the current directory).
* `mv people.csv Work` - Moves files or directories from a source to a destination. Can also handle multi-file operations or renames.
* `rm linux.txt` - Removes a standard file.
* `rm -r` - Recursively removes an entire directory and its contents.

### Bit-Level Operations (`dd`)
Used for reading and writing data at the raw bit level:

```bash
dd if=/dev/zero of=/tmp/swapex bs=1M count=50
```

* `if=` - Input file (source to read data from).
* `of=` - Output file (destination to write data to).
* `bs=` - Block Size allocation to use for the operation.
* `count=` - Total number of blocks to process.

---

## 🔍 6. Data Filtering & Regular Expressions (`grep`)
The `grep` utility acts as a powerful text filter, scanning inputs to return lines matching exact patterns (e.g., `grep sysadmin /etc/passwd` or `grep 'root' /etc/passwd`).

### Regex Pattern Syntaxes
* **Basic Patterns:**
  * `^` - Forces pattern matching only at the **beginning** of a line (e.g., `grep '^root' /etc/passwd`).
  * `$` - Forces pattern matching only at the **end** of a line (e.g., `grep 'r$' alpha-first.txt`).
  * `.` - Matches any single character (e.g., `r..f` matches four-letter character strings in order).
  * `[ ]` - Matches any single character enclosed inside the brackets (e.g., `grep '[0-9]' profile.txt`).
  * `[^ ]` - Negation; matches any single character **not** specified in the brackets (e.g., `grep '[^0-9]' profile.txt`).
  * `*` - Matches zero or more repetitions of the preceding character (e.g., `grep 're*d' red.txt` or `grep 'r[oe]*d' red.txt`).
* **Extended Patterns (`egrep` or `grep -E`):**
  * `+` - Matches one or more repetitions of the previous pattern.
  * `?` - Indicates the preceding pattern is completely optional.
  * `{ }` - Specifies a minimum, maximum, or exact count of matches.
  * `|` - Employs a logical "OR" alternation.
  * `( )` - Groups patterns together.

---

## 📥 7. Input/Output Redirection
Redirection changes where data travels by managing the three standard Linux file descriptors:
1. **STDIN (Standard Input):** Information a command receives (e.g., keyboard input like `ls ~/Documents`).
2. **STDOUT (Standard Output):** Successful command output printed to the terminal screen (e.g., typing `ls` and seeing directory paths).
3. **STDERR (Standard Error):** Error strings thrown by faulty executions (e.g., `ls fakefile` outputting `ls: cannot access fakefile: No such file or directory`).

* `>` - Redirects output streams to **overwrite** target file contents (e.g., `cat food.txt > newfile1.txt`).
* `>>` - Redirects output streams to **append** to the bottom of target files without destroying existing data.
* `echo "hello"` - Prints a specific string of text directly to the stream.

---

## ⚙️ 8. Processes, Power, & Networks

### Process Monitoring
* `ps -e` - Displays every active process on the system.
* `ps -ef` - Fetches an extended, detailed view of active system processes.
  * **PID:** Process Identifier (completely unique tracking number).
  * **TTY:** Name of the terminal window running the process.
  * **TIME:** Amount of raw processor clock-time used by the execution.
  * **CMD:** The exact command string that initiated the process.

### System & Power Management
* `date` - Prints current system calendar info, date, and clock time.
* `Ctrl + C` - Sends an interrupt signal to stop running commands and bring back a clean command prompt.
* `shutdown now` - Powers down the system immediately.
* `shutdown +1 "Goodbye World!"` - Schedules a system shutdown in +1 minute while broadcasting a custom warning string.

### Network Configurations
* `ifconfig` - Inspects or configures local network interface properties.
* `iwconfig` - Reviews dedicated wireless network interface properties.
* `ping -c 4 192.168.1.2` - Transmits exactly 4 packets to test structural network path connectivity.

---

## 📦 9. Package Management (`apt` ecosystem)
Used to manage software lifecycle on Debian-based Linux architectures:
* `apt-get update` - Synchronizes your local index logs against remote package repositories.
* `apt-cache search [keyword]` - Searches local database package descriptions for specific tools (e.g., searching for keywords like "Cow").
* `sudo apt-get install cowsay` - Downloads and installs an explicit application package. Running `cowsay` fires up the program.
* `sudo apt-get remove [package]` - Uninstalls software binaries from the machine.
* `sudo apt-get purge cowsay` - Fully purges an application package along with any leftover configuration data files.

---

## ⌨️ 10. `vi` / `vim` Text Editor Guide
A universal, terminal-bound visual text editor built into nearly every distribution. Open or create files by running: `vi newfile.txt`

### Working with Editor Modes
1. **Command Mode (Default):** Used to manipulate text, move around, and trigger actions. Pressing `Esc` at any point returns you here.
2. **Insert Mode:** Used to type data text directly.
3. **Ex Mode:** Bottom-line interface used for underlying filesystem tasks. Triggered by typing `:` from Command Mode.

### Crucial Shortcuts Matrix

| Mode | Input Command | Result / Action |
| :--- | :--- | :--- |
| **Insert Mode** | `i` / `I` | Insert text *before* cursor / at the *beginning* of the current line |
| | `a` / `A` | Insert text *after* cursor / at the *end* of the current line |
| | `o` / `O` | Open a new blank line *after* / *before* current cursor line |
| **Navigation** | `h` / `j` / `k` / `l` | Move cursor Left, Down, Up, Right (Arrow keys work too) |
| | `w` / `b` | Advance one word forward / jump one word backward |
| | `^` / `$` | Snap cursor to the very beginning / end of the current line |
| | `gg` / `G` | Jump instantly to the first line / last line of the document |
| | `[Number]G` | Jump directly to a targeted line number (e.g., `5G`) |
| | `CTRL + G` | Displays the specific line number the cursor is currently resting on |
| **Editing** | `dd` / `3dd` | Cut current line / Cut next 3 lines into system buffer clipboard |

| | dw / d3w / d4h | Cut current word / Cut next 3 words / Delete 4 characters to the left |
| | cc / cw / c3w | Change line / Change word / Change next 3 words (Deletes text + enters Insert Mode) |
| | yy / 3yy / yw / y$ | Yank (copy) current line / 3 lines / current word / text to the end of the line |
| | p / P | Put (paste) buffer clipboard data after / before the cursor location |
| Searching | /pattern | Searches forward for structural text matches. (n next match, N previous match) |
| | ?pattern | Searches backward for structural text matches. |
| Ex Mode (:)| :w / :w filename | Write (save) modifications / Save a separate backup duplicate copy as a new filename |
| | :w! | Force write modifications to system files |
| | :e filename | Open a completely separate file |
| | :q / :q! | Quit text editor / Force quit editor and discard all unsaved changes |
| | :wq | Save current modifications and quit out of the editor completely |
