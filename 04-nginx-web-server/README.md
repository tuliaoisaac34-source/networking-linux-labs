# Nginx Web Server Administration 🖥️

In this project, I practiced deploying and managing a web server using **Nginx on Ubuntu Linux through WSL**. My goal was to understand how a web server operates, how to manage Linux packages and services, and how to deploy a simple custom webpage.

This lab helped me connect Linux command-line administration with a real-world IT task: making sure a web service is running and delivering content correctly to a client.

## 🎯 Objectives

- Update Linux package repositories and install Nginx.
- Verify Nginx processes and service status.
- Create and deploy a custom HTML webpage.
- Test HTTP connectivity using command-line tools and a web browser.
- Document the process and capture evidence for troubleshooting and technical documentation.

## 🛠️ Tools and Technologies

- **Ubuntu Linux (WSL)** — Linux administration environment
- **Nginx** — Web server software
- **APT** — Linux package management
- **Nano** — Terminal-based text editor
- **Bash / Linux terminal** — Command execution and system inspection
- **Windows browser** — Website access and HTTP verification
- **GitHub** — Project documentation and portfolio management

## 🧪 Lab Implementation

### 1. Updated Package Repositories

Before installing Nginx, I refreshed the local package index to retrieve the latest available package information from the configured repositories.

```bash
sudo apt update
```

This is a common first step when preparing a Linux system for software installation.

### 2. Installed Nginx

I installed the Nginx web server using Ubuntu's package manager.

```bash
sudo apt install nginx -y
```

This installed the web server and its required dependencies.

### 3. Started and Verified the Service

I started Nginx and checked its service status to confirm that it was running.

```bash
sudo service nginx start
sudo service nginx status
```

I also inspected the running processes:

```bash
ps -ef | grep nginx
```

This helped me understand how Nginx runs in the background and how Linux process information can be used to investigate a service.

### 4. Created a Custom Webpage

I opened the default web root's HTML file using Nano:

```bash
sudo nano /var/www/html/index.html
```

I replaced the default page content with a custom landing page for my IT portfolio.

The page included a welcome message and a short description of the Linux administration skills demonstrated in this project.

This exercise introduced me to the relationship between website files and the web server responsible for delivering them to clients.

### 5. Tested HTTP Connectivity

I used the following command to check the HTTP response from the local web server:

```bash
curl -I http://localhost
```

I looked for a successful response, such as `HTTP/1.1 200 OK`.

I then opened the following address in my Windows browser:

```text
http://localhost
```

The browser test allowed me to verify the webpage from the client side, rather than relying only on terminal output.

## 📸 Lab Evidence

The following screenshots document the implementation and verification steps performed during the lab.

### Screenshot 1 — Nginx Process Verification

**Evidence:** Running the `ps -ef | grep nginx` command and inspecting the resulting process list.

This demonstrates how I checked whether Nginx processes were present in the Linux environment.

**Screenshot:**

<img width="1109" height="162" alt="01-nginx-processes" src="https://github.com/user-attachments/assets/3bf5c8a7-b5b2-4286-b0bf-2e5a2e3e1ec4" />
![Nginx process verification](screenshots/01-nginx-processes.png)

### Screenshot 2 — Custom HTML Page

**Evidence:** The custom HTML content displayed in the Nano editor before saving the file.

This demonstrates how I edited the web server's default page to deploy my own content.

**Screenshot:**

<img width="1438" height="854" alt="02-custom-html" src="https://github.com/user-attachments/assets/226bd9ba-28d1-46e0-94ed-e88649ff53f4" />
![Custom HTML source code](screenshots/02-custom-html.png)

### Screenshot 3 — Successful Browser Verification

**Evidence:** The custom webpage displayed in the browser with `http://localhost` in the address bar.

This demonstrates the client-side verification of the webpage served by Nginx.

**Screenshot:**

<img width="1439" height="850" alt="03-browser-verification" src="https://github.com/user-attachments/assets/74b69522-1a97-4b3a-996b-281855c6258a" />
![Custom Nginx webpage in browser](screenshots/03-browser-verification.png)

## 🧠 What I Learned

### Linux Package and Service Administration

I learned how to install software through APT, start a service, inspect its status, and examine the processes associated with it. These are useful foundational skills when investigating application or service issues on Linux systems.

### Web Server and Content Management

I learned that Nginx can serve static website content from a configured document root. By editing `/var/www/html/index.html`, I could change the page displayed to a browser without modifying the Nginx application itself.

### Basic Connectivity Verification

I learned to combine command-line testing with browser verification. Checking an HTTP response provides useful technical evidence, while opening the webpage confirms that the content is accessible through the browser.

### Technical Documentation

I practiced documenting commands, explaining their purpose, and organizing screenshots as evidence. Clear documentation is important when communicating troubleshooting results to teammates or recording steps for future reference.

## 🔧 Troubleshooting Notes

During similar Linux web server deployments, I would investigate the following if the webpage failed to load:

| Symptom | Initial troubleshooting step |
|---|---|
| Nginx is not running | Check `sudo service nginx status` |
| The webpage returns an error | Check the response using `curl -I http://localhost` |
| The default page still appears | Review `/var/www/html/index.html` |
| Nginx processes are not visible | Run `ps -ef \| grep nginx` |
| The browser cannot connect | Verify the service is running and test the local endpoint again |

These are troubleshooting steps to apply when the corresponding issue occurs, rather than a claim that every issue was encountered during this lab.

## ✅ Project Outcome

By completing this lab, I practiced the basic workflow of installing a Linux application, verifying its service and process state, deploying custom web content, and testing the result from a browser.

This project strengthened my foundation in Linux administration and gave me a practical example of how application services can be deployed, monitored, and verified.

**Next step:** Continue building on these fundamentals with networking, permissions, service troubleshooting, and cloud-based Linux administration.

---

*Part of my IT portfolio journey: developing practical skills in Linux, networking, cloud infrastructure, and security operations.*
