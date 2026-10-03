# 🐧 Module 01: Linux CLI Fundamentals & NDG Notes

This directory serves as the documentation repository for my foundational Linux studies through the **NDG Linux Unhatched** curriculum. It covers CLI architecture, filesystem traversal, access control, core utility filtering, and basic diagnostic commands.

---

## 📌 Core Concepts Covered

### 🖥️ Interface Paradigms
* **CLI (Command-Line Interface):** The text-based input mechanism used to directly interface with the OS kernel. Vital for Cloud/DevOps infrastructure automation.
* **GUI (Graphical User Interface):** The visual, mouse-driven window wrapper layer. 
* **Case-Sensitivity:** All Linux commands and parameters are strictly case-sensitive (e.g., `ls Documents` will succeed, but `ls documents` or `ls Documents  - Case sentsitiveopap` will fail if the case doesn't match perfectly).
* **Command Syntax Components:** 
  * `Command` (The executable action)
  * `Options/Flags` (Modifies behavior, e.g., `-l`, `-a`)
  * `Arguments` (The specific target entity the command acts upon)
* **Open Source Architecture:** Built on free, community-driven, distributable, modification-friendly core codebases.

---

## 🛠️ Detailed CLI Command Reference

### 1. Filesystem Directory Listing (`ls`)
* `ls` — Standard basic directory file listing.
* `ls -l` — Long listing display format. Exposes underlying metadata blocks:
  * *File Type Field:* `-` (Regular File), `d` (Directory), `l` (Symbolic Link).
  * *Metadata Structure:* `[Permission Blocks] [User Owner] [Group Owner] [File Size in Bytes] [Timestamp] [Filename]`.
  * *Byte Location:* Positioned directly preceding the modification timestamp field.
* `ls -a` — Exposes hidden files (all configurations prefixed with a leading dot `.`).
* `ls -t` — Sorts file outputs dynamically by chronological metadata timestamps.
* `ls -s` — Sorts file outputs by active file storage size footprint blocks.
* `ls -r` — Reverses the current structural sorting array outputs.

### 2. Navigation & Path Traversal (`pwd`, `cd`)
* `pwd` — Print Working Directory. Outputs the absolute terminal folder coordinates.
* `cd` — Change directory mapping strings.
* **Path Formats:**
  * **Absolute Paths:** Always baseline execution directly from the storage root boundary indicator (`/`) down to a target folder (e.g., `cd /home/sysadmin`).
  * **Relative Paths:** Calculations starting dynamically from your active working folder path coordinates.
* **Traversal Shortcuts:**
  * `..` — Step backward into the parent directory block.
  * `.` — Explicitly context-reference the current directory.
  * `~` — Route straight back to the active user's environment home directory.

### 3. File Operations (`cat`, `head`, `tail`, `cp`, `dd`, `mv`, `rm`)
* `cat` — Concatenate and output contents of targeted, short text files directly into stdout.
* `head -n 5` — Outputs only the top 5 targeted lines from a file text stream.
* `tail -n 10` — Defaults to outputting the final 10 tailing records of a targeted text log.
* `cp` — Duplicates files. (Example: `cp /etc/passwd .` copies systemic system data profiles straight into your active context directory folder).
* `dd` — Direct block-level bit copier utility for backup creation or storage drive partition imaging:
  * Syntax terms: `if=` (Input File target), `of=` (Output File destination), `bs=` (Defined structural block size), `count=` (Total data blocks to process).
* `mv` — Moves files or renames files inline when utilizing explicit target pathing arguments.
* `rm` — Safely deletes single standard file lines.
* `rm -r` — Recursively deletes directories and all nested structural records within them.

---

## 🔐 Administrative Privilege & Access Control

### 🛂 Identity Switching
* `su` — Switch User tool to temporarily execute commands under an alternate profile.
* `su -` / `su -l` / `su --login` — Fully initializes a clean root user environment login path shell.
* `exit` — Instantly terminates the sub-shell identity state and logs back out to the baseline profile prompt.
* `sudo` — Superuser Do. Granting restricted single task execution pathways as a root admin role configuration.

### 📄 Permission Matrix Layout
Example format string analyzed: `-rw-r--r-- 1 sysadmin sysadmin 647 Dec 20 2017 hello.sh`
* **Triad Permissions:** Read (`r`), Write (`w`), Execute (`x`).
* `chmod` — Adjusts file mode access parameters using Symbolic manipulation elements:
  * *Roles:* `u` (User), `g` (Group), `o` (Other), `a` (All).
  * *Actions:* `+` (Inject access), `-` (Strip access), `=` (Explicitly map exact permission states).
* `chown` — Transfers underlying object operational ownership groups (e.g., `sudo chown root hello.sh`).

---

## 🔍 Text Filtering & Stream Manipulation (`grep`)

Searches specified text block streams to isolate targeted matching configurations via standard or extended syntax rules:
* `grep sysadmin /etc/passwd` — Pulls user system profiles matching the identifier explicitly out of system paths.

### 🧩 Regular Expression (RegEx) Syntax Guide

| Token | Processing Type | Functional Behavior Mode |
| :--- | :--- | :--- |
| `.` | Basic | Matches any single lone character string placeholder. |
| `[ ]` | Basic | Validates a single match out of a strict bracket set choice array (e.g., `[0-9]`). |
| `[^ ]` | Basic | Inverts check, matching any character outside the bracket boundaries (e.g., `[^0-9]`). |
| `*` | Basic | Evaluates zero or more consecutive recurrences of the preceding token. |
| `^` | Basic | Anchors pattern execution strictly to the start of a code stream line (e.g., `^root`). |
| `$` | Basic | Anchors pattern evaluation tracking directly to the termination line edge (e.g., `r$`). |
| `+` | Extended (`egrep`) | Evaluates one or more occurrences of the preceding token pattern. |
| `?` | Extended (`egrep`) | Marks the preceding element string token optional. |
| `\|` | Extended (`egrep`) | Executes alternate comparison tests mirroring a logical `OR` operation block. |

---

## ⚡ System Administration & Diagnostics

### 🛑 System Controls
* `shutdown now` — Safely halts and kills standard machine running configurations immediately.
* `shutdown +1 "Goodbye World!"` — Broadcasts warning metrics before issuing delayed system drops.
* `Ctrl + C` — Sends system interrupts to instantly kill frozen foreground jobs and restore standard input command prompts.
* `date` — Outputs current system calendar configuration timeline strings.

### 🌐 Network Interfacing
* `ifconfig` / `iwconfig` — Outputs baseline active network line interface parameters or wireless network profiles.
* `ping -c 4 192.168.1.2` — Sends a discrete burst of 4 diagnostic ICMP packets to verify endpoint routing states.

### ⚙️ Process Audits (`ps`)
Exposes detailed operational tables recording active tasks mapped back to tracking fields:
* `PID` (Process Identifier tracking key), `TTY` (Terminal identifier location), `TIME` (Total system processor utilization footprint), `CMD` (Command string trigger).
* `ps -e` — Generates lists containing all ongoing background system execution paths.
* `ps -ef` — Forces long details layout parsing across all background operating process lists.
