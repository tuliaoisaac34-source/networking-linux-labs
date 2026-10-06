

<h1>Linux CLI Fundamentals & System Administration</h1>
<h3>NDG Linux Unhatched Reference & System Hardening Documentation</h3>

<p>This repository serves as a production-grade documentation matrix mapping out foundational Linux systems administration architectures covered in the NDG Linux Unhatched curriculum. It outlines CLI patterns, filesystem structures, system privilege layers, regular expression syntax matrices, and system administrative health diagnostics required to operate infrastructure within modern DevOps, Cloud Engineering, and Security workflows.</p>

<hr>

<h2>🖥️ System Architecture & Interface Paradigms</h2>

<pre><code>┌────────────────────────────────────────────────────────┐
│                      User / Shell                      │
├────────────────────────────────────────────────────────┤
│     CLI (Bash/Sh)        │        GUI (X11/Wayland)    │
├──────────────────────────┴─────────────────────────────┤
│                   Linux Kernel                         │
├────────────────────────────────────────────────────────┤
│                     Hardware                           │
└────────────────────────────────────────────────────────┘</code></pre>

<ul>
  <li><strong>CLI (Command-Line Interface):</strong> A deterministic, text-based input mechanism communicating directly with the OS kernel via a shell wrapper. Essential for automation scripts, secure remote access, and lower compute/memory resource consumption.</li>
  <li><strong>GUI (Graphical User Interface):</strong> A visual abstraction layer mapping window systems (X11/Wayland) onto terminal spaces. Unnecessary for headless enterprise server deployments.</li>
  <li><strong>Case-Sensitivity:</strong> The underlying Linux virtual filesystem layers (ext4, XFS) treat character cases distinctly at the byte level.
    <ul>
      <li><code>ls Documents</code> executes successfully.</li>
      <li><code>ls documents</code> fails with a 'No such file or directory' error if the lowercase target folder does not exist.</li>
    </ul>
  </li>
  <li><strong>Open Source Governance:</strong> Distributed under the <strong>GNU General Public License (GPL)</strong>, ensuring codebase audatability, open redistribution architectures, and deep infrastructure flexibility.</li>
</ul>

<h3>🧩 Command Syntax Mechanics</h3>
<p>Linux shell parsers evaluate terminal strings according to a strict three-part hierarchical sequence:</p>

<pre><code>[ Command ] ───► [ Options / Flags ] ───► [ Arguments ]</code></pre>

<ol>
  <li><strong>Command:</strong> The primary executable binary or built-in shell tool triggered (e.g., <code>ls</code>).</li>
  <li><strong>Options/Flags:</strong> Switched modifiers prefixed with a hyphen (<code>-</code>) to alter processing behaviors (e.g., <code>-l</code>, <code>-a</code>). Multiple flags can be grouped into an efficient singular block (e.g., <code>-la</code>).</li>
  <li><strong>Arguments:</strong> The target entities or path elements upon which the command acts (e.g., <code>/var/log</code>).</li>
</ol>

<hr>

<h2>🛠️ Detailed CLI Command Matrix</h2>

<h3>1. Filesystem Directory Listing (<code>ls</code>)</h3>
<p>Exposes operational metadata records bound to object directories.</p>

<ul>
  <li><code>ls</code> — Outputs a simple layout grid of visible folder contents.</li>
  <li><code>ls -l</code> — Invokes long-listing format, breaking down systemic security and storage allocations:</li>
</ul>

<pre><code>-rw-r--r--  1  sysadmin  sysadmin  4096  Oct 06 09:26  production.log
▲└───┬───┘  ▲      ▲         ▲       ▲        ▲               ▲
│    │      │      │         │       │        │               └─ Filename
│    │      │      │         │       │        └─ Modification Timestamp
│    │      │      │         │       └─ File Size in Bytes
│    │      │      │         └─ Group Owner
│    │      │      └─ User Owner
│    │      └─ Hard Link Count
│    └─ Permissions Triad (User, Group, Other)
└─ File Type Indicator ( - = Regular File, d = Directory, l = Symbolic Link )</code></pre>

<ul>
  <li><code>ls -a</code> — Exposes all items including hidden dotfiles prefixed with a leading period (<code>.</code>), such as configuration scripts (<code>.bashrc</code>).</li>
  <li><code>ls -t</code> — Orders outputs chronologically based on file modification timestamps.</li>
  <li><code>ls -S`</code> — Orders outputs by storage capacity size footprints.</li>
  <li><code>ls -r</code> — Reverses the current sorting hierarchy (e.g., <code>ls -laSr</code> lists smallest to largest hidden entries).</li>
</ul>

<h3>2. Navigation & Path Traversal (<code>pwd</code>, <code>cd</code>)</h3>
<p>Provides mechanics for shifting contextual execution tracks across the filesystem tree.</p>

<ul>
  <li><code>pwd</code> — <em>Print Working Directory</em>. Evaluates and outputs the absolute shell environment coordinate tracking string from the system root.</li>
  <li><code>cd</code> — <em>Change Directory</em>. Re-maps active working coordinate sets.
    <ul>
      <li><strong>Absolute Paths:</strong> Paths evaluated directly from the system root (<code>/</code>) down to a target child node, invariant of current location (e.g., <code>cd /var/log/nginx</code>).</li>
      <li><strong>Relative Paths:</strong> Paths evaluated relative to the active working terminal coordinate context (e.g., if inside <code>/var</code>, running <code>cd log/nginx</code>).</li>
    </ul>
  </li>
  <li><strong>Navigation Shortcuts:</strong>
    <ul>
      <li><code>..</code> — Traverses exactly one hierarchy level backward into the parent directory.</li>
      <li><code>.</code> — References the immediate working directory context explicitly.</li>
      <li><code>~</code> — Resolves dynamically to the logged-in user's system home directory path (<code>/home/$USER</code>).</li>
      <li><code>-</code> — Toggles back to the previous working directory context.</li>
    </ul>
  </li>
</ul>

<h3>3. File Operations & Stream Analysis</h3>

<h4>Content Inspection</h4>
<ul>
  <li><code>cat</code> — <em>Concatenate</em>. Streams complete raw data sequences from specified text files directly to standard output (<code>stdout</code>). Best reserved for short configuration profiles.</li>
  <li><code>head -n [X]</code> — Limits standard output to the exact first <code>X</code> lines of a file stream (defaults to 10 if <code>-n</code> is omitted).</li>
  <li><code>tail -n [X]</code> — Limits standard output to the final <code>X</code> lines of a file stream.
    <ul>
      <li><code>tail -f</code> — <em>Follow mode</em>. Keeps the file stream open dynamically to print incoming lines in real-time. Essential for live log troubleshooting.</li>
    </ul>
  </li>
  <li><code>less</code> — Interactive terminal pager utility. Allows backward and forward scrolling navigation through large log structures without loading the entire asset block into system memory.</li>
</ul>

<h4>Data Manipulation & Block Copying</h4>
<ul>
  <li><code>touch</code> — Instantly creates an empty file if the target does not exist, or updates the access and modification timestamps of an existing file.</li>
  <li><code>cp</code> — Copies file arrays across target folder routes.
    <ul>
      <li><code>cp -r</code> — Recursively copies directories, maintaining structural hierarchies.</li>
      <li><em>Usage Example:</em> <code>cp /etc/passwd .</code> (Copies system data into the active working directory).</li>
    </ul>
  </li>
  <li><code>mv</code> — Moves files across filesystem endpoints. Also performs atomic inline renames when destination targets remain within localized directories.</li>
  <li><code>rm</code> — Permanently purges standard files from the filesystem index.
    <ul>
      <li><code>rm -r</code> — Recursively deletes directories and all nested children data structures.</li>
      <li><code>rm -f`</code> — Overrides interactive confirmations, forcing immediate elimination. <strong>Warning:</strong> Irreversible in standard environments.</li>
    </ul>
  </li>
  <li><code>dd</code> — Low-level, block-by-block bitstream duplicator. Used for bare-metal backups, partition cloning, and master boot record isolation.
    <ul>
      <li><em>Syntax Parameters:</em> <code>if=</code> (Input Device), <code>of=</code> (Output Device Target), <code>bs=</code> (Block Size), <code>count=</code> (Total blocks).</li>
      <li><em>Production Command:</em> <code>sudo dd if=/dev/sda of=/backup/disk_image.raw bs=4M</code></li>
    </ul>
  </li>
</ul>

<hr>

<h2>🔐 Administrative Privilege & Access Control</h2>

<h3>🛂 Identity Switching Mechanics</h3>
<p>Linux maintains strict isolation between standard user profiles and the administrative root operating layer.</p>

<ul>
  <li><code>su</code> — <em>Switch User</em>. Switches the active shell context to an alternate profile. Requires target user's password.</li>
  <li><code>su -</code> (or <code>su -l</code>) — Invokes a complete login shell transformation. Purges current environmental variables and instantiates the target profile's explicit environment variables and configurations.</li>
  <li><code>sudo</code> — <em>Superuser Do</em>. Executes a single target operation utilizing the elevated privilege scopes of the root environment based on configurations inside the <code>/etc/sudoers</code> safety policy layout. Requires the <em>current</em> user's password, reducing shared credential risks.</li>
  <li><code>exit</code> — Terminates the active sub-shell instance, dropping the session back down to the preceding profile prompt level.</li>
</ul>

<h3>📄 Permission Matrix Layout & Modification</h3>
<p>File attributes map directly to a strict access matrix split among three functional entities: <strong>User (u)</strong>, **Group (g)**, and **Other (o)**.</p>

<h4>Octal vs. Symbolic Permission Architecture</h4>
<p>Permissions translate between alphabetic characters and binary weight positions:</p>

<table>
  <thead>
    <tr>
      <th>Permission</th>
      <th>Character</th>

Octal Calculation Matrix Example:
r w x │ r - x │ r - -
4+2+1 │ 4+0+1 │ 4+0+0
(7) │ (5) │ (4) --> Resulting Mode: 754
Modifying Permissions (chmod, chown)

chmod — Change Mode. Adjusts security flags on assets.

Symbolic Assignment: chmod g+w,o-r security_profile.txt
Octal Assignment: chmod 755 deployment.sh


chown — Change Owner. Assigns alternative user and group operational ownership layers.

Production Command: sudo chown root:sysadmin infrastructure.conf


🔍 Text Filtering & Stream Manipulation (grep)
The grep (Global Regular Expression Print) engine filters data streams to locate and output explicit patterns parsed out of standard input or plain-text files.
Usage Example: grep 'sysadmin' /etc/passwd (Extracts target identity parameters from system user records).
grep -i — Skips case distinction constraints entirely during scanning sweeps.
grep -v — Inverts the filter, returning only lines that do not match the pattern.
grep -E — Activates Extended Regular Expression features (equivalent to using egrep).
🧩 Regular Expression (RegEx) Reference Matrix
⚡ System Administration & Diagnostics
🛑 System Lifecycle & Foreground Control

shutdown now — Signals immediate execution of systemic operational shifts, safely halting services and cutting mainboard power.
shutdown +1 "Emergency Maintenance Inbound" — Broadcasts system-wide warning banners to active terminals before enforcing a 1-minute delayed drop cycle.
Ctrl + C — Broadcasts an explicit SIGINT (Signal Interrupt) vector to active foreground processes, terminating run states immediately.
date — Standard output format showing current system calendar and timezone metrics.
🌐 Network Diagnostics & Interface Topology

ifconfig — Outputs detailed structural mapping parameters covering network interfaces, IP bindings, and MAC IDs.
iwconfig — Dedicated wireless interface utility exposing link qualities and radio configurations.
ping -c 4 1.1.1.1 — Dispatches exactly 4 network diagnostic ICMP Echo Request frames to verify endpoint connectivity.
⚙️ Process Audits & Resource Tracking (ps)
Exposes detailed operational allocation records mapping active kernel processes back to tracking IDs.
Process Table Structure Metadata Fields:

PID: Process Identifier. The discrete numerical tracking address assignment key used by the kernel.
TTY: TeleTypewriter. Identifies the explicit control terminal channel managing the operation.
TIME: Total cumulative execution processor utilization clock blocks.
CMD: The specific command binary instantiation call string that triggered the process.


ps — Standard display loop showing active tasks running inside the user's current terminal instance.
ps -e — Pulls data strings mapping every active runtime execution thread throughout the system namespace.
ps -ef — Forces full details layout parsing (-f) across all background processes.


<FollowUp>
Check attached photos for reference </FollowUp>


