# EL_Task1
Task1 (Nmap Scanning)
# scanning
Learn to discover open ports on devices in your local network to understand network exposure.

# Local Network Port Scanning Guide

A hands-on guide to discovering open ports on your local network using Nmap. This project is designed for educational purposes to help you understand your network's exposure and basic security posture.

---


## 🎯 Objective

The goal of this exercise is to learn how to discover open ports on various devices (computers, phones, smart TVs, IoT devices, etc.) within your local network. By identifying these open ports, you can begin to understand the services running on your network and assess potential security risks.

---

## 🛠️ Tools Required

*   **Nmap (Network Mapper)**: An essential, free, and open-source tool for network discovery and security auditing.
    *   **Download**: (https://nmap.org/download.html)
*   **Wireshark (Optional)**: A powerful network protocol analyzer that lets you see what's happening on your network at a microscopic level.
    *   **Download**: (https://www.wireshark.org/download.html)

---

## 📝 Step-by-Step Guide

### Step 1: Install Nmap

Download and install Nmap for your operating system (Windows, macOS, or Linux) from the official website linked above. The installer will typically add Nmap to your system's PATH, allowing you to run it from any terminal or command prompt.

### Step 2: Find Your Local IP Range

Before you can scan, you need to know what to scan. You must identify the IP address range of your local network.

*   **On Windows:**
    Open Command Prompt (`cmd.exe`) and type:
    ```sh
    ipconfig
    ```
    Look for your "Wireless LAN adapter" or "Ethernet adapter". Note the `IPv4 Address` (e.g., `192.168.1.15`) and the `Subnet Mask` (e.g., `255.255.255.0`). A subnet mask of `255.255.255.0` means your network range is `192.168.1.0/24`.

*   **On macOS or Linux:**
    Open a terminal and type:
    ```sh
    ifconfig
    # or a more modern command:
    ip a
    ```
    Look for your active network interface (e.g., `en0` on Mac, `eth0` or `wlan0` on Linux). Find the `inet` address. If your IP is `192.168.1.15`, your network range is likely `192.168.1.0/24`. The `/24` is CIDR notation for the `255.255.255.0` subnet mask and means Nmap will scan all 256 addresses in that range (from `192.168.1.0` to `192.168.1.255`).

### Step 3: Run the Nmap Scan

Now, run the Nmap scan. We will use the `-sS` flag, which performs a TCP SYN scan (also known as a "half-open" or "stealth" scan). It's fast and less likely to be logged by target systems.

Open your terminal or command prompt and run the following command, **replacing `192.168.1.0/24` with your actual network range**.

```sh
nmap -sS 192.168.1.0/24
```
Nmap will now send packets to every potential host on your network and report back which hosts are up and which of their ports are open. This may take a few minutes.

### Step 4: Research common services running on those ports

🔹 Port 53 (TCP)
Service: DNS (Domain Name System)

Used By: DNS servers (like BIND or Microsoft DNS)

Function: Resolves domain names (like google.com) to IP addresses (like 142.250.195.78).

Common Usage:

Used by routers, DNS servers, and internal network services.

Usually runs over UDP, but TCP is used for larger queries or zone transfers.

Security Concerns:

Open TCP 53 can be abused for DNS amplification attacks or data exfiltration.

Should be monitored and restricted if not required for external queries.

🔹 Port 135 (TCP)
Service: Microsoft RPC (Remote Procedure Call)

Used By: Windows systems for DCOM, WMI, and various service communications.

Function: Facilitates communication between Windows components, including remote management tasks.

Common Usage:

Used by Windows services like Windows Management Instrumentation (WMI).

Security Concerns:

Historically targeted by malware (e.g., MSBlaster worm).

Should be restricted to trusted hosts only within a LAN.

🔹 Port 139 (TCP)
Service: NetBIOS Session Service

Used By: Older versions of Windows file and printer sharing.

Function: Allows computers to share files and printers over a local network.

Common Usage:

Used in Windows networks for backward compatibility.

Security Concerns:

May expose shared folders or usernames if unprotected.

Often disabled in modern networks in favor of SMB over port 445.

🔹 Port 445 (TCP)
Service: SMB (Server Message Block) / Microsoft-DS

Used By: Windows File Sharing, Active Directory, and printer sharing.

Function: Allows access to shared files, printers, and named pipes over the network.

Common Usage:

Used heavily in Windows networking for sharing files and domain controller communication.

Security Concerns:

Frequently targeted by ransomware (like WannaCry).

Should be disabled or firewalled off from public internet.

Always keep SMB services up-to-date.

✅ Summary Table
Port	Protocol	Common Service	Purpose	Risk Level	Recommendation
53	TCP	DNS	Resolves domain names	Medium	Restrict access if not a DNS server
135	TCP	Microsoft RPC	Windows inter-process calls	High	Block externally, use firewall
139	TCP	NetBIOS Session	Legacy Windows sharing	Medium	Disable if not needed
445	TCP	SMB / Microsoft-DS	Windows file and printer share	High	Disable on public, patch regularly
