# Linux Networking & Cisco Fundamentals 🌐

Welcome to my Linux Networking repository! This project documents **Week 3** of my IT portfolio journey. It connects foundational Cisco networking principles with hands-on Linux terminal utilities to diagnose, document, and troubleshoot network path failures.

---

## 📡 1. Cisco Networking Foundations
Before troubleshooting a connectivity issue, an administrator must understand how data physically and structurally traverses an infrastructure environment.

### Network Classifications
* **LAN (Local Area Network):** Interconnects localized network computing devices within close proximity, such as a home, building, or office.
* **MAN (Metropolitan Area Network):** Covers a larger geographical space spanning across a town or an entire city infrastructure.
* **WAN (Wide Area Network):** Telecommunications network that connects distant regions, states, or countries across global distances.

### Physical Network Media Specifications
* **Twisted-pair (Copper):** Operates at speeds up to 1 Gbps with a maximum cable limit of 100 meters (least expensive solution).
* **Fiber Optic:** High-performance media executing transmission speeds at 10 Gbps+ spanning long-distance routes up to 60 kilometers (most expensive).
* **Wireless:** Radiates local connectivity metrics averaging 54 Mbps out to a 100-meter threshold (moderate deployment cost).

---

## 💻 2. Linux Network Configuration Commands
These core terminal utilities are used to view local interface hardware parameters and test layer connectivity mapping.

* **ifconfig** – Interface Configuration utility used to view active network details (such as IP addresses, subnet masks, and interface states) or manually assign link parameters.
* **iwconfig** – Dedicated configuration command for managing wireless network interface components (such as SSID, frequency channels, and data rates).
* **ping -c 4 <destination>** – Transmits a sequence of 4 ICMP Echo Request packets to verify point-to-point network-layer connectivity with a target IP host or domain name.

---

## 🧪 3. Hands-on Linux, Networking, and Nginx Troubleshooting Lab

### 🚪 The Investigative Flow
When an infrastructure connection falls over or an endpoint is isolated, we isolate faults progressively from the local machine out to the application layer:
```text
💻 COMPUTER ➔ 🌐 IP ADDRESS ➔ 🚪 DEFAULT GATEWAY ➔ 🔎 DNS ➔ 🔌 PORT ➔ 🖥️ SERVICE
```

### Problem Scenario
A user reports that the local web application server hosted on the network is completely inaccessible and failing to load.

### Step-by-Step Investigation & Resolution

#### 1️⃣ Step 1: Verify Local Network Configurations
Check if the local machine has a properly assigned IP address and that the interfaces are up.
```bash
# Check wired interface settings and local IP provisioning
ifconfig

# Verify wireless adapter links and SSID bindings if on Wi-Fi
iwconfig
```
* **Result:** Local interfaces are operational and configured with valid IP addresses.

#### 2️⃣ Step 2: Test Local Gateway Path Connectivity
Isolate whether the connection drop is internal to the workstation or a failure on the upstream router infrastructure.
```bash
# Ping your Local Default Gateway address (the router door)
ping -c 4 192.168.1.1
```
* **Result:** Gateway responds successfully with 0% packet loss.

#### 3️⃣ Step 3: Test Target Nginx Host Network Reachability
Attempt to contact the specific network target hosting the web server application.
```bash
# Execute a targeted 4-packet ICMP ping to the Nginx server IP address
ping -c 4 192.168.1.2
```
* **Evidence:** The terminal returns `Destination Host Unreachable` or a 100% packet loss metric summary. 

#### 4️⃣ Step 4: Root Cause Identification & Fix
* **Root Cause:** Based on the Cisco media guidelines, the physical copper drop running to the Nginx host exceeded the 100-meter cable restriction length, resulting in severe signal degradation and a dropped link layer state on the remote switch interface.
* **Solution:** Re-routed the network path using a certified twisted-pair cable under the 100-meter threshold to restore the hardware link layer state.

#### 5️⃣ Step 5: Verification
```bash
# Re-test point-to-point network connectivity to the Nginx host
ping -c 4 192.168.1.2
```
* **Result:** ICMP transmissions are successful with a 0% packet loss response. The web server interface is reachable again.

---

## 📸 Lab Evidence
* **Screenshot 1 — Interface Status Check:** *(Add image showing your `ifconfig` output displaying your local IP parameters)*
* **Screenshot 2 — Failed Investigation Ping:** *(Add image showing the `ping -c 4` attempt to the Nginx host failing with unreachable errors)*
* **Screenshot 3 — Fixed Connectivity Verification:** *(Add image showing the successful 4-packet ping transmission after resolving the infrastructure issue)*

---

## 🧠 What I Learned
* **Isolating the Layer:** Instead of modifying Nginx application configurations immediately, following the logical flow saved time by proving the root problem sat purely at the physical and network layers.
* **Physical Constraints Matter:** Knowing Cisco media limits—like the 100-meter maximum length for copper twisted-pair cabling—helps system administrators look beyond software configurations when troubleshooting intermittent network dropouts.
