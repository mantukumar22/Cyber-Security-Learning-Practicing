# Computer Networking Notes — 2027 Edition
### For Ethical Hackers & Security Professionals

> Scope: This document is built the way a security professional studies networking — fundamentals first, then the security/offensive-security context layered on top. Tools and attack types are explained conceptually (what they are, how they work, how to defend against them) so this is safe to use for study, interviews, and certification prep (Network+, Security+, CCNA, CEH). Always test, scan, or "attack" only systems you own or are explicitly authorized to test in writing — unauthorized access to networks/systems is illegal in virtually every jurisdiction.

---

## Table of Contents
1. [Networking Models](#1-networking-models)
2. [IP Addressing](#2-ip-addressing)
3. [Networking Hardware](#3-networking-hardware)
4. [Topologies](#4-topologies)
5. [Transmission Media](#5-transmission-media)
6. [Transport Layer: TCP vs UDP](#6-transport-layer-tcp-vs-udp)
7. [Core Protocols & Ports Reference](#7-core-protocols--ports-reference)
8. [Address Resolution & Routing](#8-address-resolution--routing)
9. [Wireless Networking](#9-wireless-networking)
10. [Core Security Concepts](#10-core-security-concepts)
11. [Security Appliances & Technologies](#11-security-appliances--technologies)
12. [Common Network-Based Threats](#12-common-network-based-threats-defensive-overview)
13. [Ethical Hacking Methodology](#13-ethical-hacking-methodology)
14. [Modern & Cloud Networking](#14-modern--cloud-networking)
15. [2027 Outlook — What's Changing](#15-2027-outlook--whats-changing)
16. [Quick-Reference Glossary](#16-quick-reference-glossary)
17. [Further Learning Paths](#17-further-learning-paths)

---

## 1. Networking Models

### 1.1 OSI Model (7 Layers)
A conceptual model describing how data moves through a network. It's mostly a teaching/troubleshooting tool — real traffic doesn't strictly respect these boundaries, but "what layer is this problem at?" is the first question every network engineer and pentester asks.

| # | Layer | Definition | Examples / Protocols | Security Relevance |
|---|-------|------------|----------------------|---------------------|
| 7 | Application | Where user-facing software talks to the network | HTTP, FTP, SMTP, DNS | App-layer attacks: SQLi, XSS, business logic abuse |
| 6 | Presentation | Translates, encrypts, compresses data into a usable format | SSL/TLS, JPEG, ASCII | Encryption/decryption happens here |
| 5 | Session | Establishes, manages, tears down sessions between hosts | NetBIOS, RPC, sockets | Session hijacking targets this layer |
| 4 | Transport | End-to-end delivery, reliability, flow control | TCP, UDP | Port scanning, SYN floods, segmentation |
| 3 | Network | Logical addressing and routing between networks | IP, ICMP, routers | IP spoofing, routing attacks |
| 2 | Data Link | Physical addressing within a local segment, framing | Ethernet, MAC, switches, ARP | ARP spoofing, MAC flooding, VLAN hopping |
| 1 | Physical | Raw bits over a physical medium | Cables, radio waves, NICs | Wiretapping, physical access attacks |

Mnemonic: **"Please Do Not Throw Sausage Pizza Away"** (Physical → Application).

### 1.2 TCP/IP Model (4–5 Layers)
The model the actual internet runs on. Simpler and more practical than OSI.

| TCP/IP Layer | Maps to OSI | Purpose |
|---|---|---|
| Application | 5–7 | User protocols (HTTP, DNS, SSH, etc.) |
| Transport | 4 | TCP/UDP — reliability and ports |
| Internet | 3 | IP addressing and routing |
| Link (Network Access) | 1–2 | Physical transmission and MAC addressing |

---

## 2. IP Addressing

### 2.1 IPv4
- **Definition:** A 32-bit address (e.g., `192.168.1.10`), written as four octets (0–255) separated by dots.
- **Classes (legacy, now mostly replaced by CIDR):**

| Class | Range | Default Mask | Use |
|---|---|---|---|
| A | 1.0.0.0 – 126.255.255.255 | /8 | Huge networks |
| B | 128.0.0.0 – 191.255.255.255 | /16 | Medium networks |
| C | 192.0.0.0 – 223.255.255.255 | /24 | Small networks (most common today) |
| D | 224.0.0.0 – 239.255.255.255 | n/a | Multicast |
| E | 240.0.0.0 – 255.255.255.255 | n/a | Experimental/reserved |

- **Private (RFC1918) ranges — never routed on the public internet:**
  - `10.0.0.0/8`
  - `172.16.0.0/12`
  - `192.168.0.0/16`
- **CIDR notation:** `/24` means the first 24 bits are the network portion. `192.168.1.0/24` = 256 addresses (254 usable hosts after network + broadcast).
- **Subnetting quick reference:**

| CIDR | Subnet Mask | Usable Hosts |
|---|---|---|
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /26 | 255.255.255.192 | 62 |
| /27 | 255.255.255.224 | 30 |
| /28 | 255.255.255.240 | 14 |
| /30 | 255.255.255.252 | 2 (point-to-point links) |

### 2.2 IPv6
- **Definition:** A 128-bit address written in 8 groups of hex digits (e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`), built because IPv4's ~4.3 billion addresses are effectively exhausted given IoT/mobile growth.
- **Shorthand rules:** leading zeros in a group can be dropped, and one run of consecutive all-zero groups can be collapsed to `::` (only once per address).
- **Address types:**
  - **Unicast** — one sender, one receiver.
  - **Multicast** — one sender, many receivers (replaces broadcast entirely in IPv6).
  - **Anycast** — one address shared by multiple hosts; traffic goes to the nearest one (used heavily by CDNs and DNS root servers).
  - **Link-local** (`fe80::/10`) — auto-assigned, not routable beyond the local segment.
- **Why it matters by 2027:** IPv4 exhaustion plus IoT growth keeps pushing IPv6 adoption; expect dual-stack environments to remain the norm, but more ISPs and cloud providers defaulting to IPv6-only internally with NAT64/464XLAT for legacy compatibility.

### 2.3 NAT & PAT
- **NAT (Network Address Translation):** Translates private IPs to a public IP so internal devices can reach the internet without each needing a public address.
- **PAT (Port Address Translation / "NAT overload"):** The common form of NAT where many internal devices share one public IP, distinguished by port number.
- **Security note:** NAT is *not* a substitute for a firewall — it provides incidental obscurity, not real access control.

---

## 3. Networking Hardware

| Device | Definition | Security Note |
|---|---|---|
| **Hub** | Dumb Layer 1 device — repeats every signal to every port (broadcasts to all). Largely obsolete. | Anything connected can sniff all traffic on the segment. |
| **Switch** | Layer 2 device that forwards frames only to the port owning the destination MAC address (via a MAC address table). | Vulnerable to MAC flooding (forces it into "fail-open" broadcast mode) and VLAN hopping. |
| **Router** | Layer 3 device that forwards packets between different networks based on IP addresses. | Misconfigured ACLs/routing are a common attack surface. |
| **Bridge** | Connects and filters traffic between two LAN segments at Layer 2. | Mostly replaced by switches today. |
| **Gateway** | Any device/node that connects two dissimilar networks (often the router at a network's edge). | Often the chokepoint for perimeter security controls. |
| **Access Point (AP)** | Bridges wireless clients to a wired network. | Rogue APs and evil-twin attacks exploit weak AP management. |
| **Firewall** | Filters traffic based on rules (see Section 11). | Core perimeter control — frequently the first thing pentesters try to identify and fingerprint. |
| **Load Balancer** | Distributes traffic across multiple servers for performance/availability. | Can also terminate TLS — a high-value target if compromised. |
| **Proxy / Reverse Proxy** | Proxy: forwards client requests outward, hides client identity. Reverse proxy: sits in front of servers, hides server identity, often does TLS termination, caching, WAF functions. | Misconfigured proxies can leak internal network details (SSRF risk). |

---

## 4. Topologies

| Topology | Definition |
|---|---|
| **Bus** | All devices share a single central cable. Simple, but one break kills the segment. |
| **Star** | All devices connect to a central switch/hub. Most common LAN design today. |
| **Ring** | Each device connects to exactly two neighbors, forming a loop. Used historically (Token Ring, FDDI). |
| **Mesh** | Every device connects to every other device (full mesh) or to several others (partial mesh). High redundancy, used in WANs and resilient infrastructure. |
| **Hybrid/Tree** | Combination of the above — most real-world enterprise networks. |

---

## 5. Transmission Media

| Medium | Definition | Notes |
|---|---|---|
| **Twisted Pair (Cat5e/6/6a/8)** | Copper cabling; pairs twisted to reduce crosstalk. | Cat6a/Cat8 support 10 Gbps+ at shorter distances. |
| **Fiber Optic** | Light pulses through glass/plastic strands. Single-mode (long distance, laser) vs multi-mode (shorter distance, LED). | Immune to electromagnetic interference; harder (not impossible) to tap without detection. |
| **Coaxial** | Copper core with shielding; legacy cable internet/TV. | Mostly phased out for new deployments. |
| **Wireless (RF)** | Radio frequency transmission — Wi-Fi, cellular, Bluetooth. | Inherently broadcast; anyone in range can capture traffic, making encryption essential. |

---

## 6. Transport Layer: TCP vs UDP

| | TCP | UDP |
|---|---|---|
| **Definition** | Connection-oriented, reliable transport protocol | Connectionless, "best effort" transport protocol |
| **Handshake** | 3-way handshake (SYN, SYN-ACK, ACK) | None |
| **Reliability** | Guarantees delivery and order (retransmits lost packets) | No guarantees — faster but can drop/reorder packets |
| **Overhead** | Higher (sequencing, acknowledgment, flow control) | Lower |
| **Use Cases** | Web browsing, email, file transfer — anything needing accuracy | Video/voice calls, DNS lookups, gaming — anything needing speed |
| **Security Relevance** | SYN flood attacks abuse the handshake | Easier to spoof source IP since there's no handshake to verify |

---

## 7. Core Protocols & Ports Reference

| Port | Protocol | Transport | Purpose | Security Note |
|---|---|---|---|---|
| 20/21 | FTP | TCP | File transfer (control/data) | Sends credentials in cleartext — use SFTP/FTPS instead |
| 22 | SSH / SFTP | TCP | Encrypted remote login & file transfer | Standard secure replacement for Telnet/FTP |
| 23 | Telnet | TCP | Unencrypted remote login | Cleartext — should be disabled on modern networks |
| 25 | SMTP | TCP | Sending email | Port 587 (with STARTTLS) is the secure modern standard |
| 53 | DNS | TCP/UDP | Domain name resolution | Targeted by spoofing/cache poisoning; DoH/DoT add encryption |
| 67/68 | DHCP | UDP | Automatic IP address assignment | Rogue DHCP servers can redirect traffic |
| 69 | TFTP | UDP | Simple file transfer, no auth | Used for firmware/config — should be tightly restricted |
| 80 | HTTP | TCP | Unencrypted web traffic | Should redirect to HTTPS in any modern deployment |
| 110/995 | POP3 / POP3S | TCP | Retrieving email | 995 is the TLS-encrypted version |
| 123 | NTP | UDP | Time synchronization | Can be abused for DDoS amplification |
| 143/993 | IMAP / IMAPS | TCP | Retrieving email (syncs across devices) | 993 is the TLS-encrypted version |
| 161/162 | SNMP | UDP | Network device monitoring/management | v1/v2c send community strings in cleartext — use SNMPv3 |
| 389/636 | LDAP / LDAPS | TCP | Directory services (e.g., Active Directory lookups) | 636 is the TLS-encrypted version |
| 443 | HTTPS | TCP | Encrypted web traffic (TLS) | The modern baseline for all web traffic |
| 445 | SMB | TCP | Windows file/printer sharing | Historically a major attack surface (e.g., EternalBlue-class exploits) |
| 514 | Syslog | UDP | Centralized log collection | Sent in cleartext by default; pair with TLS-syslog where possible |
| 3389 | RDP | TCP | Windows Remote Desktop | Frequently targeted by brute-force/ransomware actors if exposed directly to the internet |

---

## 8. Address Resolution & Routing

- **ARP (Address Resolution Protocol):** Maps an IP address to a MAC address on a local segment ("who has this IP?"). Foundational to LAN communication and the basis of ARP spoofing attacks.
- **Static Routing:** Manually configured routes. Predictable, doesn't scale well.
- **Dynamic Routing:** Routers exchange information automatically via routing protocols.

| Routing Protocol | Type | Notes |
|---|---|---|
| RIP | Distance-vector | Simple, old, limited to small networks (max 15 hops) |
| OSPF | Link-state | Common in enterprise internal networks |
| EIGRP | Advanced distance-vector (Cisco) | Fast convergence, Cisco-specific |
| BGP | Path-vector | Routes traffic *between* organizations — runs the internet's backbone |

---

## 9. Wireless Networking

### 9.1 Wi-Fi Standards (802.11)

| Standard | Marketing Name | Max Theoretical Speed | Band |
|---|---|---|---|
| 802.11n | Wi-Fi 4 | ~600 Mbps | 2.4/5 GHz |
| 802.11ac | Wi-Fi 5 | ~3.5 Gbps | 5 GHz |
| 802.11ax | Wi-Fi 6 / 6E | ~9.6 Gbps | 2.4/5/6 GHz |
| 802.11be | Wi-Fi 7 | ~46 Gbps (theoretical) | 2.4/5/6 GHz |

By 2027, Wi-Fi 7 hardware is mainstream in new enterprise and consumer gear, and 6 GHz spectrum use is widespread where regulators have opened it.

### 9.2 Wireless Security Standards

| Standard | Definition | Status |
|---|---|---|
| **WEP** | Original Wi-Fi encryption, uses weak RC4 + static keys | Broken — never use |
| **WPA** | Interim fix to WEP using TKIP | Deprecated |
| **WPA2** | Uses AES-CCMP encryption; introduced 4-way handshake | Still common, but vulnerable to offline dictionary attacks on weak passphrases |
| **WPA3** | Adds Simultaneous Authentication of Equals (SAE), resists offline brute-force, forward secrecy | Current standard; should be the default for any new deployment |

### 9.3 Other Short-Range Protocols
- **Bluetooth** — short-range (~10m typical) device pairing; BLE (Bluetooth Low Energy) dominates IoT.
- **NFC (Near-Field Communication)** — very short range (cm), used in contactless payments and access badges.
- **Zigbee/Z-Wave** — low-power mesh protocols common in smart-home/IoT devices.

---

## 10. Core Security Concepts

- **CIA Triad:**
  - **Confidentiality** — only authorized parties can read data.
  - **Integrity** — data isn't altered without detection.
  - **Availability** — systems/data are accessible when needed.
- **AAA Framework:**
  - **Authentication** — proving identity (passwords, MFA, certificates).
  - **Authorization** — what an authenticated identity is allowed to do.
  - **Accounting** — logging what was done, by whom, when.
- **Defense in Depth:** Layering multiple independent security controls so no single failure compromises the whole system.
- **Least Privilege:** Every user/system gets only the access strictly necessary to do its job.
- **Network Segmentation / VLANs:** Splitting a network into isolated zones (e.g., guest Wi-Fi separate from corporate Wi-Fi) so a breach in one zone doesn't automatically expose others.
- **Zero Trust Architecture (ZTA):** "Never trust, always verify" — no implicit trust is granted based on network location alone; every request is authenticated, authorized, and encrypted regardless of whether it originates inside or outside the traditional perimeter. This has become the dominant enterprise security model heading into 2027.

---

## 11. Security Appliances & Technologies

| Technology | Definition |
|---|---|
| **Packet-Filtering Firewall** | Inspects packet headers (IP/port) against static rules; no awareness of connection state. |
| **Stateful Firewall** | Tracks the state of active connections, allowing return traffic for established sessions automatically. |
| **Next-Gen Firewall (NGFW)** | Adds application-awareness, intrusion prevention, and identity-based rules on top of stateful filtering. |
| **WAF (Web Application Firewall)** | Filters HTTP(S) traffic specifically, blocking attacks like SQL injection and XSS at the application layer. |
| **IDS (Intrusion Detection System)** | Monitors traffic and *alerts* on suspicious activity. Passive. |
| **IPS (Intrusion Prevention System)** | Monitors traffic and *actively blocks* malicious activity in real time. |
| **VPN (Virtual Private Network)** | Creates an encrypted tunnel over an untrusted network. Common implementations: **IPsec** (network-layer, common for site-to-site), **SSL/TLS VPN** (application-layer, browser/client-based), **WireGuard** (modern, lightweight, increasingly the default for new deployments due to simplicity and speed). |
| **SIEM (Security Information and Event Management)** | Aggregates and correlates logs across the environment to detect and investigate incidents. |
| **NAC (Network Access Control)** | Enforces policy on devices before/while they connect to the network (e.g., checking for up-to-date antivirus). |
| **DMZ (Demilitarized Zone)** | A buffer network segment, isolated from the internal LAN, where internet-facing services live so a compromise there doesn't directly expose internal systems. |

---

## 12. Common Network-Based Threats (Defensive Overview)

> Explained conceptually for awareness and defense — these are standard CEH/Security+ topics, not exploitation guides.

| Threat | What It Is | Primary Defense |
|---|---|---|
| **Sniffing/Eavesdropping** | Capturing traffic on a network segment (trivial on hubs/wireless, harder on switched networks). | Encryption (TLS everywhere), switched networks, port security. |
| **Man-in-the-Middle (MITM)** | An attacker secretly intercepts/relays traffic between two parties who believe they're communicating directly. | TLS with certificate validation, mutual authentication, VPNs. |
| **ARP Spoofing** | An attacker sends forged ARP replies to associate their own MAC address with another host's IP, redirecting traffic through them. | Dynamic ARP Inspection (DAI), static ARP entries, port security on switches. |
| **DNS Spoofing/Cache Poisoning** | Injecting false DNS responses so victims are redirected to malicious destinations. | DNSSEC, DoH/DoT, validating resolvers. |
| **IP Spoofing** | Forging the source IP address of packets, often to bypass IP-based access controls or hide attacker identity. | Ingress/egress filtering (BCP38), anti-spoofing ACLs. |
| **DoS/DDoS** | Overwhelming a target with traffic/requests so legitimate users can't get service; "distributed" means many sources at once. | Rate limiting, scrubbing services/CDNs, anycast, capacity planning. |
| **Port Scanning** | Probing a host's ports to discover open services — typically reconnaissance, not an attack in itself. | Firewalls, minimizing exposed services, IDS/IPS alerting on scan patterns. |
| **Session Hijacking** | Stealing or predicting a valid session token to impersonate an authenticated user. | Short-lived tokens, TLS, secure cookie flags, re-authentication for sensitive actions. |
| **Rogue Access Point / Evil Twin** | An unauthorized AP (sometimes mimicking a legitimate SSID) used to intercept wireless clients. | Wireless intrusion detection, certificate-based Wi-Fi auth (802.1X/EAP-TLS), client awareness. |

---

## 13. Ethical Hacking Methodology

Authorized penetration testing follows a structured process — this is the standard framework taught across CEH, PTES, and OSCP-aligned training.

1. **Pre-engagement / Scoping** — written authorization (rules of engagement), defined scope, legal sign-off. This step is non-negotiable; testing without explicit authorization is illegal.
2. **Reconnaissance** — passive (OSINT, public records, DNS lookups) and active (direct probing) information gathering about the target.
3. **Scanning & Enumeration** — identifying live hosts, open ports, and running services (e.g., `nmap -sV <target>` to fingerprint service versions on an authorized target).
4. **Vulnerability Assessment** — matching discovered services/versions against known vulnerabilities (tools like Nessus or OpenVAS).
5. **Exploitation** — *only within authorized scope* — attempting to leverage a vulnerability to dem
