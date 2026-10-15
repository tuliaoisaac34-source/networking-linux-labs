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

<img width="791" height="41" alt="01-ip-configuration" src="https://github.com/user-attachments/assets/24663b68-4361-49cf-bed8-5345576b6908" />

### 2. Test Network Connectivity

**Command:**

```powershell
ping 1.1.1.1
```

**Purpose:**
- Test ICMP reachability to an external IP address.
- Observe response times and packet loss.

A failed ping does not automatically prove that internet connectivity is unavailable because ICMP traffic may be filtered.

<img width="539" height="233" alt="02-ping-test" src="https://github.com/user-attachments/assets/5ce12d61-00e8-4d18-b39d-1c1070ca182f" />

### 3. Inspect the Default Route

**Command:**

```powershell
Get-NetRoute -DestinationPrefix "0.0.0.0/0"
```

**Purpose:**
- Identify configured IPv4 default routes.
- Inspect the next-hop address and interface.
- Compare routing information with the IP configuration.

<img width="695" height="131" alt="03-routing-table" src="https://github.com/user-attachments/assets/fbce731a-1079-447c-b4d2-02091e99a223" />

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

<img width="939" height="497" alt="04-tcp-connections" src="https://github.com/user-attachments/assets/7a05110c-ab08-4ca4-ad1b-52ea632a0f6b" />


### 5. Test DNS Resolution

**Command:**

```powershell
Resolve-DnsName example.com
```

**Purpose:**
- Query DNS records for a hostname.
- Determine whether the hostname resolves to IP addresses.
- Collect evidence when investigating a possible name-resolution issue.

<img width="888" height="176" alt="05-dns-resolution" src="https://github.com/user-attachments/assets/a0dd4aa5-f7c0-4290-9d29-a18966f225a5" />

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

<img width="599" height="187" alt="06-port-test" src="https://github.com/user-attachments/assets/2409ff77-3187-40dd-ab46-6d3274adf504" />

### 7. Test an HTTP Response

**Command:**

```powershell
curl.exe -I --max-time 10 https://example.com
```

**Purpose:**
- Send an HTTP HEAD request.
- Inspect the returned HTTP status and headers.
- Check whether the web endpoint responds to the request.

<img width="645" height="265" alt="07-http-test" src="https://github.com/user-attachments/assets/e81bcdb7-a085-4fed-a0ba-fb0cd3f7eb5b" />

Investigation and findings:

<img width="1347" height="711" alt="07-1 Investigation and findings" src="https://github.com/user-attachments/assets/7f7874c2-b91d-4319-a24b-c203fe207941" />

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

<img width="885" height="465" alt="08-packet-tracer-topology" src="https://github.com/user-attachments/assets/ba15bfdc-0b04-4b0a-8b0f-3b1c6f867ea7" />

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

<img width="1141" height="591" alt="09-baseline-connectivity" src="https://github.com/user-attachments/assets/03497cd5-f3b0-4145-9687-d3d4f46cf87b" />

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

<img width="1128" height="584" alt="10-http-failure" src="https://github.com/user-attachments/assets/ef0d181f-c3d5-40c3-8de1-773bdbb8668d" />

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

<img width="1060" height="551" alt="11-http-recovery" src="https://github.com/user-attachments/assets/e9094373-a215-4cac-a049-f1be817fad25" />
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

**Status:**  Completed

All findings and screenshots should reflect the tests actually performed.
