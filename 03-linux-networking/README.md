# 🌐 Week 3 — Windows Networking & Cisco Fundamentals

## 🎯 Project Overview

This project demonstrates foundational network troubleshooting using **Windows PowerShell** and **Cisco Packet Tracer**.

The goal is to investigate connectivity issues systematically, distinguish network-layer problems from application-service failures, document the root cause, apply a fix, and verify recovery.

**Skills Demonstrated:**
- Windows network configuration and troubleshooting
- IPv4 addressing and routing fundamentals
- ICMP connectivity testing
- DNS resolution
- TCP port connectivity testing
- HTTP service verification
- Cisco Packet Tracer network simulation
- Root-cause analysis and technical documentation

## 🛠️ Tools and Technologies

| Tool | Purpose |
|---|---|
| Windows PowerShell | Network diagnostics and connectivity testing |
| Cisco Packet Tracer | Simulated network topology and fault investigation |
| GitHub | Project documentation and evidence |

## 📂 Repository Structure

```text
03-linux-networking/
├── README.md
├── screenshots/
│   ├── 01-ip-configuration.png
│   ├── 02-ping-test.png
│   ├── 03-routing-table.png
│   ├── 04-tcp-connections.png
│   ├── 05-dns-resolution.png
│   ├── 06-port-test.png
│   ├── 07-http-test.png
│   ├── 08-packet-tracer-topology.png
│   ├── 09-baseline-connectivity.png
│   ├── 10-http-failure.png
│   └── 11-http-recovery.png
└── notes/
    └── troubleshooting-report.md
```

**Where to put the screenshots**

Save all screenshots inside the `screenshots/` folder located in `03-linux-networking/`.

For example, the full path for the first screenshot should be:

`03-linux-networking/screenshots/01-ip-configuration.png`

Use the exact filenames listed above so the image links in this README work correctly. Capture your own terminal output and Packet Tracer results.

## 🧪 Lab 1 — Windows PowerShell Network Diagnostics

### Objective

Inspect the local network configuration and test connectivity, DNS resolution, TCP ports, and HTTP responses using built-in Windows tools.

### 1. Inspect IP Configuration

**Command:**

```powershell
ipconfig /all
```

**Purpose:**
- Identify network adapters.
- Inspect IPv4 addresses and subnet masks.
- Identify the default gateway.
- Review DNS server configuration.

**Screenshot:** `screenshots/01-ip-configuration.png`

![PowerShell IP configuration](screenshots/01-ip-configuration.png)

### 2. Test Network Connectivity

**Command:**

```powershell
ping 1.1.1.1
```

**Purpose:**
- Test ICMP reachability to an external IP address.
- Observe response times and packet loss.

A failed ping does not automatically prove that internet connectivity is unavailable because ICMP traffic may be filtered.

**Screenshot:** `screenshots/02-ping-test.png`

![PowerShell ping test](screenshots/02-ping-test.png)

### 3. Inspect the Default Route

**Command:**

```powershell
Get-NetRoute -DestinationPrefix "0.0.0.0/0"
```

**Purpose:**
- Identify configured IPv4 default routes.
- Inspect the next-hop address and interface.
- Compare routing information with the IP configuration.

**Screenshot:** `screenshots/03-routing-table.png`

![PowerShell routing table](screenshots/03-routing-table.png)

### 4. Inspect TCP Connections and Listening Ports

**Commands:**

```powershell
Get-NetTCPConnection |
    Select-Object -First 20
```

```powershell
Get-NetTCPConnection -State Listen
```

**Purpose:**
- Inspect existing TCP connections.
- Identify locally listening TCP ports.
- Practice examining network socket information.

These commands show local Windows TCP information; they do not directly confirm that a remote server is listening.

**Screenshot:** `screenshots/04-tcp-connections.png`

![PowerShell TCP connections](screenshots/04-tcp-connections.png)

### 5. Test DNS Resolution

**Command:**

```powershell
Resolve-DnsName example.com
```

**Purpose:**
- Query DNS records for a hostname.
- Determine whether the hostname resolves to IP addresses.
- Collect evidence when investigating a possible name-resolution issue.

**Screenshot:** `screenshots/05-dns-resolution.png`

![PowerShell DNS resolution](screenshots/05-dns-resolution.png)

### 6. Test TCP Port Connectivity

**Command:**

```powershell
Test-NetConnection example.com -Port 443
```

**Purpose:**
- Test TCP connectivity to the destination on port 443.
- Inspect the destination address and connection result.
- Determine whether the tested TCP connection succeeded.

Look for:

```text
TcpTestSucceeded : True
```

A successful result confirms TCP connectivity to that destination and port at the time of testing. It does not guarantee that the website itself is functioning correctly.

**Screenshot:** `screenshots/06-port-test.png`

![PowerShell TCP port test](screenshots/06-port-test.png)

### 7. Test an HTTP Response

**Command:**

```powershell
curl.exe -I --max-time 10 https://example.com
```

**Purpose:**
- Send an HTTP HEAD request.
- Inspect the returned HTTP status and headers.
- Check whether the web endpoint responds to the request.

**Screenshot:** `screenshots/07-http-test.png`

![PowerShell HTTP test](screenshots/07-http-test.png)

## 🧪 Lab 2 — Cisco Packet Tracer Troubleshooting

### Objective

Simulate a small office network and investigate a website outage caused by an intentionally disabled HTTP service.

### 1. Build the Network Topology

Create a topology containing:

- PC0 — Client workstation
- Switch0 — Access switch
- Router0 — Default gateway
- Server0 — Web server

Connect the devices using compatible Ethernet interfaces and Copper Straight-Through cables.

**Screenshot:** `screenshots/08-packet-tracer-topology.png`

![Cisco Packet Tracer topology](screenshots/08-packet-tracer-topology.png)

### 2. Configure the Network

Use the following IPv4 addressing plan:

| Device | IPv4 Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC0 | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` |
| Router0 | `192.168.10.1` | `255.255.255.0` | Not required for this LAN interface |
| Server0 | `192.168.10.20` | `255.255.255.0` | `192.168.10.1` |

Configure Router0's LAN interface with the gateway address and enable the interface.

Example commands, assuming the interface is `GigabitEthernet0/0`:

```text
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
end
show ip interface brief
```

Configure PC0 and Server0 through their respective **Desktop → IP Configuration** menus.

On Server0, open **Services → HTTP** and set HTTP to **On**.

### 3. Verify Baseline Connectivity

On PC0, open **Desktop → Command Prompt** and run:

```text
ping 192.168.10.1
ping 192.168.10.20
```

Then open **Desktop → Web Browser** and visit:

```text
http://192.168.10.20
```

Confirm that the gateway and server respond and that the web page loads before introducing the fault.

**Screenshot:** `screenshots/09-baseline-connectivity.png`

![Packet Tracer baseline connectivity](screenshots/09-baseline-connectivity.png)

### 4. Reproduce the Website Failure

On Server0:

1. Open **Services → HTTP**.
2. Change HTTP from **On** to **Off**.
3. Return to PC0.
4. Ping the server again:

```text
ping 192.168.10.20
```

5. Open the browser and revisit:

```text
http://192.168.10.20
```

Expected observation:

- The server remains reachable by IP.
- The website no longer loads while HTTP is disabled.

**Screenshot:** `screenshots/10-http-failure.png`

![Packet Tracer HTTP failure](screenshots/10-http-failure.png)

### 5. Identify the Root Cause

The controlled test shows that the server remains reachable at the network layer, but the web application becomes unavailable when its HTTP service is disabled.

**Root cause:** The HTTP service was intentionally switched off in the Packet Tracer server configuration.

This finding is supported by the simulated test and does not imply a real-world DNS failure, routing failure, or physical cable problem.

### 6. Restore the Service and Verify Recovery

On Server0:

1. Open **Services → HTTP**.
2. Change HTTP back to **On**.
3. Return to PC0.
4. Repeat the server ping:

```text
ping 192.168.10.20
```

5. Reopen the browser:

```text
http://192.168.10.20
```

Confirm that the web page loads again.

**Screenshot:** `screenshots/11-http-recovery.png`

![Packet Tracer HTTP recovery](screenshots/11-http-recovery.png)

## 🔎 Troubleshooting Methodology

The investigation follows this general sequence:

```text
Computer
   ↓
IP Configuration
   ↓
Default Gateway
   ↓
Destination IP
   ↓
DNS Resolution
   ↓
TCP Port
   ↓
Application Service
   ↓
Verification
```

The sequence is a diagnostic guide rather than a rigid rule. For example, a website that is accessible by IP but not by hostname may require DNS investigation, while a reachable server with an unavailable website may require port and service checks.

## 🧠 Key Learnings

- Inspected Windows IP configuration and routing information.
- Practiced ICMP connectivity testing and packet-loss interpretation.
- Used PowerShell to inspect TCP connections and listening ports.
- Tested DNS resolution and TCP port connectivity.
- Inspected HTTP responses from a web endpoint.
- Built and configured a simulated office network.
- Reproduced an application-service failure in Cisco Packet Tracer.
- Applied a targeted fix and verified recovery.
- Documented findings using an evidence-based troubleshooting process.

## ⚠️ Lab Limitations

PowerShell diagnostics were performed on the Windows host. The controlled HTTP failure was reproduced separately in Cisco Packet Tracer.

The Packet Tracer scenario is a simulation. It does not establish that the Windows host experienced the same outage or that a production network has the same configuration.

## ✅ Completion Checklist

- [ ] Executed the PowerShell diagnostics.
- [ ] Recorded actual command results.
- [ ] Captured and saved the PowerShell screenshots.
- [ ] Built the Packet Tracer topology.
- [ ] Verified baseline connectivity.
- [ ] Reproduced the HTTP service failure.
- [ ] Restored HTTP and verified recovery.
- [ ] Saved all screenshots in the `screenshots/` directory.
- [ ] Verified that all README image links work.
- [ ] Reviewed the repository for confidential information.

## 📌 Project Status

**Status:** In progress / Completed

All findings and screenshots should reflect the tests actually performed.
