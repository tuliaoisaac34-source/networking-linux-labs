# Nginx Web Server Administration 🖥️

Welcome to my Nginx Web Server repository! This project documents **Week 4** of my IT portfolio journey. It focuses on package installation, background process verification, custom web site creation, and the logical application flow from a client browser over port 80 down to the web daemon.

---

## ⚙️ 1. Nginx Service Components & Architecture
An administrator must understand the operational flow required to deliver web data from a server application layer over to a client endpoint device:

```text
💻 Client Browser ➔ 🌐 Host IP Address ➔ 🔌 HTTP Port 80 ➔ 🖥️ Nginx Daemon ➔ 📄 Web Page (index.html)
```

### Core Administrative Tools Reviewed:
* **apt-get update** – Synchronizes local repository information with official online software package archives.
* **apt-cache search nginx** – Searches the package manager database to locate installation binaries.
* **sudo apt-get install nginx** – Downloads, satisfies software dependencies, and deploys the Nginx web server engine on the system.
* **vi** / **vim** – The terminal-based text editor utilized to modify standard web page index structures and update local server configuration layouts.

---

## 🧪 2. Hands-on Laboratory: Deploying a Custom Nginx Web Server

### Objective
Install the Nginx web package, verify its status, deploy a custom HTML landing page by editing configuration zones using the `vi` editor, and test endpoint rendering.

---

### 🚀 Step-by-Step Lab Execution

#### Step 1: Synchronize Package Management Index
Before deploying a new application service, refresh your local package cache definitions.
```bash
sudo apt-get update
```

#### Step 2: Query and Deploy Nginx
Locate and install the web server core binary dependencies.
```bash
# Search for Nginx strings within repositories
apt-cache search nginx

# Install the Nginx web package
sudo apt-get install nginx
```

#### Step 3: Verify Application Process States
Confirm that the new web server daemon is active and running across system process identifiers.
```bash
# Display an extended details layout of all running system processes
ps -ef | grep nginx
```

#### Step 4: Provision Your Custom Web Site Data
Navigate to the standard default web directory and modify the landing page data. We will utilize **Vi editor shortcuts** to completely replace the standard screen template text.
```bash
# Change directory to the web root space
cd /var/www/html/

# Open the main deployment file with administrative root permissions
sudo vi index.html
```

**Vi Operational Workflow Applied:**
1. Pressed **`Esc`** followed by **`3dd`** to cut and clear out the old placeholder title lines.
2. Pressed **`i`** to enter **Insert Mode** and typed out the custom web design layout string below:
   ```html
   <h1>Welcome to My First IT Portfolio Web Server!</h1>
   <p>This page confirms Nginx is operational and running smoothly.</p>
   ```
3. Pressed **`Esc`** to return to **Command Mode**.
4. Typed **`:wq`** and pressed Enter to save the content and safely exit the file workspace.

#### Step 5: Verify Interface Address Parameters
Locate your server's active network IP binding configuration to browse the website.
```bash
ifconfig
```

---

## 📸 Lab Evidence & Screen Captures

### 🔲 Screenshot 1 — Service Process Verification
*Run `ps -ef | grep nginx` to prove your server process background daemons are actively running on the machine.*
*(Attach your terminal screenshot here)*

### 🔲 Screenshot 2 — File Modification inside Vi
*Capture a picture of your custom HTML code structure sitting actively inside your open `vi index.html` terminal window before saving.*
*(Attach your terminal screenshot here)*

### 🔲 Screenshot 3 — Successful Browser Delivery
*Open your web browser tool and type your server's IP address (found via `ifconfig`) into the URL bar to confirm your custom website text displays perfectly.*
*(Attach your web browser screen capture here)*

---

## 🧠 What I Learned
* **Decoupling Content From System:** I learned that Nginx handles data delivery by looking at a distinct configuration path layout (`/var/www/html/`). This means I can change the website files easily without messing up the main program settings.
* **Process Tracking Over Service Commands:** By utilizing `ps -ef` to search for active PIDs rather than relying on automatic modern wrapper status commands, I gained deep clarity into how Linux manages master and worker background tasks at a lower operating system level.

