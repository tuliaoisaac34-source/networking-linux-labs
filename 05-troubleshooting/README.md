# Linux & Service Troubleshooting 🛠️

Welcome to my Troubleshooting repository! This section marks the culmination of **Week 4 and 5** of my portfolio journey. In this folder, I document my hands-on **Break/Fix Labs**—where components are intentionally broken, logically investigated, and safely resolved using low-level Linux auditing tools.

---

## 🧭 The Troubleshooting Methodology
To solve IT infrastructure bugs efficiently without guessing, I follow a strict, structured investigation framework:

```text
🚨 PROBLEM ➔ 🔍 INVESTIGATION ➔ 📸 EVIDENCE ➔ 🧠 ROOT CAUSE ➔ 🛠️ SOLUTION ➔ ✅ VERIFICATION
```

---

## 🧪 Hands-on Break/Fix Lab: Nginx Service Down

### 🚨 1. Problem Profile
* **Symptom:** A remote client attempts to view the portfolio website by putting the server's IP address into their web browser, but the connection times out completely. The site fails to load.

---

### 🔍 2. Investigation Steps

#### Step A: Validate Local Interface IP Configuration
First, confirm that the server still possesses its correct, active network identity.
```bash
ifconfig
```
* *Result:* The network interface is up and displaying the correct IP address bindings.

#### Step B: Audit Active Background Network Daemons
Check if the web server daemon is actively tracking and processing system transactions.
```bash
ps -ef | grep nginx
```
* *Result:* The terminal only prints out the `grep` command process string itself. The core Nginx master and worker daemon processes are entirely missing from the active system process table.

#### Step C: Inspect the Application Log for Errors
Since the process is not running, navigate to the system logging directory to find out why Nginx failed or stopped.
```bash
cd /var/log/nginx/
cat error.log | grep -E 'error|crit|emerg'
```
* *Result:* The log files reveal that an administrator modified the core configuration file layout using an unsupported parameters string.

#### Step D: Audit the Configuration File Using Vi
Open the main web environment layout file to track down the configuration typo.
```bash
sudo vi /etc/nginx/nginx.conf
```
* *Investigation Action inside Vi:* Used the **`/`** shortcut command in Command Mode (e.g., `/port`) to search forward through text lines until finding the damaged parameter block.

---

### 📸 3. Lab Evidence
* **Screenshot 1 — Missing Processes:** *(Attach screenshot of running `ps -ef | grep nginx` showing that no Nginx master or worker PIDs are running)*
* **Screenshot 2 — Configuration Typo inside Vi:** *(Attach screenshot of your `vi /etc/nginx/nginx.conf` layout showing the broken line of code before you fixed it)*
* **Screenshot 3 — Successful Process Restoration:** *(Attach screenshot of running `ps -ef | grep nginx` after your fix, proving the background PIDs are active again)*

---

### 🧠 4. Root Cause
An administrative update was performed on the configuration file (`/etc/nginx/nginx.conf`) using the `vi` editor. During the update, an invalid block character was typed into the file, causing the Nginx master process to crash and fail to bind to its listening port during system operation.

---

### 🛠️ 5. Solution
1. Opened the configuration file using root permissions: `sudo vi /etc/nginx/nginx.conf`.
2. Located the broken text parameter block line using the Vi forward search utility (`/`).
3. Switched Vi into **Insert Mode** by pressing **`i`** and deleted the typo character.
4. Pressed **`Esc`** to enter Command Mode, typed **`:wq`**, and pressed Enter to save modifications and quit the text environment.
5. Fired up the application core binary from the terminal root context to restore normal operations.

---

### ✅ 6. Verification
To guarantee that the fix is permanent and stable, audit the process environment and verify local network loopback connectivity:
```bash
# Verify Nginx background processes are up with unique PIDs
ps -ef | grep nginx

# Run an internal test to verify port responses
ping -c 4 127.0.0.1
```
* **Final Result:** The Nginx processes are fully visible in the active process table, and the portfolio landing page loads perfectly in the web browser tool with a 0% packet loss response.

---

## 🧠 What I Learned
* **Log Files Are the Roadmap:** Instead of randomly reinstalling packages using `apt-get` when a system breaks, checking `/var/log/` using tools like `cat` and `grep` points directly to the exact file and line number causing the issue.
* **Process vs. Service Awareness:** Monitoring infrastructure with `ps -ef` shows the true state of a system. A configuration file might look correct, but checking for active Process Identifiers (PIDs) proves whether the operating system is actually executing the program.

