# Linux & Service Troubleshooting 🛠️

In this project, I practiced troubleshooting a Linux web server by investigating a stopped Nginx service and diagnosing an intentionally introduced configuration error. The goal was to understand how to identify service problems, inspect system processes, analyze configuration errors, and safely restore normal operation.

Rather than immediately reinstalling software or guessing at the cause, I followed a structured troubleshooting process: **investigate, identify the root cause, apply a fix, and verify the result.**

## 🎯 Objectives

- Identify whether Nginx is running or stopped.
- Inspect Linux processes to determine whether the web server is active.
- Diagnose a configuration error using Nginx's built-in configuration test.
- Review application logs for additional troubleshooting information.
- Correct the issue without modifying the main Nginx configuration unnecessarily.
- Restore the service and verify HTTP connectivity.
- Document the troubleshooting process with screenshots and command-line evidence.

## 🛠️ Tools and Technologies

- **Ubuntu Linux (WSL)** — Environment for Linux administration and troubleshooting
- **Nginx** — Web server used for the lab
- **Bash / Linux terminal** — Command execution and system inspection
- **Nano** — Terminal-based text editor
- **`ps`** — Process inspection
- **`nginx -t`** — Configuration syntax validation
- **`tail`** — Log inspection
- **`curl`** — HTTP response testing
- **GitHub** — Technical documentation and portfolio management

## 🧭 Troubleshooting Methodology

I followed a structured approach to avoid making assumptions about the cause of a service problem.

1. **Identify the problem** — Determine whether the website or service is available.
2. **Investigate** — Inspect the service status, running processes, configuration, and logs.
3. **Identify the root cause** — Use the available evidence to locate the configuration issue.
4. **Apply a fix** — Remove the invalid test configuration.
5. **Verify recovery** — Confirm that Nginx starts and responds to HTTP requests.
6. **Document the result** — Capture screenshots and record the commands used.

## 🧪 Hands-on Lab: Nginx Service Troubleshooting

### 1. Established a Baseline

Before introducing the test failure, I checked the current Nginx service status and verified that the local web server was responding.

```bash
sudo service nginx status
curl -I http://localhost
```

A successful baseline helped establish that the web server was working before the troubleshooting exercise began.

### 2. Simulated a Service Failure

To practice investigating a service that was no longer running, I intentionally stopped Nginx.

```bash
sudo service nginx stop
```

I then inspected the process list:

```bash
ps -ef | grep '[n]ginx'
```

With Nginx stopped, no Nginx master or worker processes should appear in the results. This provided evidence that the service was not running.

### 3. Introduced and Identified a Configuration Error

To avoid damaging the main Nginx configuration, I created a separate temporary configuration file for the test.

```bash
sudo nano /etc/nginx/conf.d/lab-broken.conf
```

I added an intentionally invalid directive:

```nginx
this_directive_does_not_exist;
```

After saving the file, I ran the Nginx configuration test:

```bash
sudo nginx -t
```

The configuration test reported an error associated with the invalid directive. This demonstrated how configuration validation can help identify a specific problem before attempting to start or reload a service.

I also reviewed the Nginx error log:

```bash
sudo tail -n 30 /var/log/nginx/error.log
```

The configuration test was the primary diagnostic tool for this particular error. The log review provided additional context about the server's operation.

### 4. Applied the Fix

After identifying the invalid test configuration, I removed the temporary file:

```bash
sudo rm /etc/nginx/conf.d/lab-broken.conf
```

I validated the remaining configuration:

```bash
sudo nginx -t
```

A successful test should report that the syntax is okay and the configuration test passed.

I then started Nginx again:

```bash
sudo service nginx start
```

This approach allowed me to correct the issue without unnecessarily reinstalling Nginx or changing the main configuration file.

### 5. Verified Service Recovery

After restarting Nginx, I inspected the process list again:

```bash
ps -ef | grep '[n]ginx'
```

I then tested the HTTP response:

```bash
curl -I http://localhost
```

Finally, I opened the local website in my browser:

```text
http://localhost
```

A successful HTTP response and the return of the custom webpage provided evidence that the service was operational again.

## 📸 Lab Evidence

The following screenshots document the main stages of the troubleshooting exercise.

### Screenshot 1 — Nginx Processes Before Recovery

**Objective:** Demonstrate the absence of Nginx master and worker processes while the service is stopped.

I used `ps -ef | grep '[n]ginx'` to inspect the process list and confirm that Nginx was not running.

<img width="1128" height="242" alt="01-nginx-processes-down" src="https://github.com/user-attachments/assets/79adec81-a91e-49f9-916a-61b71f6f4616" />

### Screenshot 2 — Configuration Error Investigation

**Objective:** Document the intentionally invalid configuration directive before removing it.

This screenshot demonstrates the configuration error used in the lab. The `sudo nginx -t` command was used to validate the configuration and identify the invalid directive.

<img width="1103" height="624" alt="02-nginx-config-error" src="https://github.com/user-attachments/assets/3057dca6-1905-42ae-b8ad-163a222662fd" />

### Screenshot 3 — Nginx Processes After Recovery

**Objective:** Demonstrate that Nginx processes returned after the invalid configuration was removed and the service was started again.

I used the process list to verify that Nginx was running after the fix.

**Screenshot:**

<img width="1102" height="213" alt="03-nginx-processes-restored" src="https://github.com/user-attachments/assets/9ce3bab1-37e8-4d83-a17f-0e6c683ea6a3" />

## 🔍 Troubleshooting Reference

| Symptom | Diagnostic command | Purpose |
|---|---|---|
| Nginx is stopped | `sudo service nginx status` | Check service status |
| Nginx processes are missing | `ps -ef \| grep '[n]ginx'` | Inspect running processes |
| Configuration test fails | `sudo nginx -t` | Identify configuration syntax errors |
| Additional diagnostic information is needed | `sudo tail -n 30 /var/log/nginx/error.log` | Review recent error log entries |
| Website does not respond as expected | `curl -I http://localhost` | Inspect the HTTP response |

These commands form a useful starting point for investigating Linux web server problems. The appropriate next step depends on the output and the specific symptoms observed.

## 🧠 What I Learned

### 1. Verify Before Making Changes

A service that is unavailable can have several possible causes. Checking its status and running processes helps narrow down the investigation before making changes.

### 2. Configuration Validation Saves Time

The `nginx -t` command provides a direct way to validate Nginx configuration syntax. In this lab, it identified the invalid directive, allowing me to focus on the actual problem instead of reinstalling software unnecessarily.

### 3. Logs and Processes Provide Different Evidence

Process inspection shows whether an application is running, while logs can provide additional information about errors and service behavior. Using both helps build a clearer picture of a problem.

### 4. Verify the Fix From the Client Side

Restarting a service is not enough by itself. Testing the HTTP response and opening the website in a browser helps confirm that the application is responding as expected.

### 5. Protect the Existing Environment

Using a separate test configuration allowed me to practice failure and recovery without intentionally corrupting the main Nginx configuration. This reinforced the importance of controlled testing and minimizing unnecessary changes.

## ✅ Project Outcome

This project gave me hands-on practice with a structured Linux troubleshooting workflow. I simulated a stopped Nginx service, introduced and diagnosed a configuration error, removed the invalid configuration, and checked whether the service could be restored.

These are foundational skills relevant to IT support, Linux administration, application troubleshooting, and cloud infrastructure operations.

**Next step:** Continue building practical experience with Linux permissions, networking diagnostics, service logs, and cloud-hosted web servers.

---

*Part of my IT portfolio journey: developing practical skills in Linux administration, networking, cloud infrastructure, and security operations.*
