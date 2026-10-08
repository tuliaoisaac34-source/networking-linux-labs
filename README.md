# Hands-On Linux, Networking, and Nginx Troubleshooting Labs 🐧🌐

Welcome to my IT infrastructure portfolio! This repository serves as a documented, public record of my technical competencies developed across Linux system administration, Cisco core networking fundamentals, and web application server operations.

The contents of this project demonstrate my practical ability to navigate the command-line interface (CLI), manage account security, analyze files with regular expressions, deploy services, and systematically troubleshoot infrastructure issues using a logical diagnostic framework.

---

## 🛠️ Core Technologies & Concepts Practiced
* **Operating Systems:** Ubuntu Linux Environment / Command-Line Interface (CLI).
* **Identity & Access Management:** User contexts, administrative elevation (`su`, `sudo`), and symbolic permission mapping (`chmod`, `chown`).
* **Networking Foundations:** Cisco hardware media specifications (Copper/Fiber/Wireless metrics) and local network layer validation tools.
* **Web Services:** Nginx application delivery mechanics and application process management.
* **Text Editing & Data Manipulation:** Configuration file alterations using `vi` and pattern isolation via regular expressions (`grep`).

---

## 🗂️ Repository Structure & Lab Overview

### 📁 [01-linux-basics](./01-linux-basics/)
Focuses on fundamental CLI terminal maneuvers, folder structuring, and text workflows.
* **Key Commands:** `pwd`, `cd`, `ls -la`, `mkdir`, `cp`, `mv`, `rm`, `cat`, `less`, `head`, `tail`.
* **Mini Lab:** Provisioned a custom file workspace layout (`~/october-lab/`) to practice relative pathing, directory tree generation, and text inspections. Includes full documentation of the terminal **Vi text editor shortcuts** for file modification.

### 📁 [02-linux-administration](./02-linux-administration/)
Covers system identity status checks, file system security controls, and package distributions.
* **Key Commands:** `su -`, `sudo`, `passwd -S`, `chown`, `chmod` (Symbolic Method), `ps -ef`, `apt-get`.
* **Mini Lab:** Applied the **Principle of Least Privilege** by elevating to a root shell context, locking file ownership, and tweaking symbolic user/group/other properties to secure application scripts from unauthorized users.

### 📁 [03-linux-networking](./03-linux-networking/)
Integrates **Cisco Basic Networking course foundations** with Linux network utilities.
* **Key Concepts:** LAN/MAN/WAN topologies, physical media thresholds (100m copper limits), interface checking (`ifconfig`, `iwconfig`), and ICMP diagnostics (`ping -c 4`).
* **Mini Lab:** Walked through a progressive connectivity fault isolation lab mapping out the logical troubleshooting chain from the local computer interface out to a remote gateway and host IP destination.

### 📁 [04-nginx-web-server](./04-nginx-web-server/)
Covers deploying a functional web service application and editing web root directory components.
* **Key Actions:** `apt-get update`, `apt-get install nginx`, `ps -ef`, and writing clean HTML with the `vi` editor.
* **Mini Lab:** Fully deployed an Nginx daemon web server layer, validated active background worker PIDs, and customized the default landing page template data (`index.html`) using raw console text actions over Port 80.

### 📁 [05-troubleshooting](./05-troubleshooting/)
Demonstrates the critical system auditing and engineering skills necessary for an IT support role.
* **Methodology:** Problem ➔ Investigation ➔ Evidence ➔ Root Cause ➔ Solution ➔ Verification.
* **Mini Lab:** Analyzed an intentional **Break/Fix Scenario** where an invalid configuration typo crashed the web application. Successfully inspected system logs using `grep`, fixed code blocks using `vi`, and verified background process identifier (PID) restoration.

---

## 🧭 My Troubleshooting Methodology
Whenever an infrastructure asset or application service drops offline, I avoid guessing and follow this strict diagnostic workflow to pinpoint the root cause:

1. **Problem:** Identify and define the active system symptoms reported by the user or monitoring tools.
2. **Investigation:** Follow a progressive flow from the local machine out to the application layer (`ifconfig` ➔ Gateway ➔ Ping Host ➔ App Process check).
3. **Evidence:** Capture raw terminal data, error codes, log errors, and active process maps (`ps -ef`).
4. **Root Cause:** Determine the exact baseline failure mechanism (e.g., misconfigured parameter or physical length issue).
5. **Solution:** Formulate and execute a target system fix to rectify the infrastructure bug.
6. **Verification:** Re-test network path transmissions and service availability to confirm stability.

---

## 📈 Next Milestones
* [x] Week 1: Master Linux CLI & Workspace Navigation
* [x] Week 2: User Identity & Symbolic Access Security 
* [x] Week 3: Fusing Linux Tools with Cisco Networking Rules
* [x] Week 4: Nginx Web Server Deployments & Intentionally Breaking Services
* [ ] Next Phase: Automating tasks, expanding labs, and building Portfolio #2!

