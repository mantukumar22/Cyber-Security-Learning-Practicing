# Day 2 - NMAP

**Notes link:** [Nmap Tutorial Part 2](https://sheryians.notion.site/Nmap-Tutorial-Part-2-3041267520c780509f22de7c852fe59f?source=copy_link)

---

## 1. Timing Templates

Check: [nmap.org - Timing Templates](https://nmap.org/book/performance-timing-templates.html)

### Nmap Timing Templates (`-T0` to `-T5`)

Templates control the speed/stealth trade-off of a scan — how aggressively Nmap sends probes and how long it waits for responses. In real engagements, picking the right template matters as much as picking the right scan type.

### The Six Templates

| Template | Name | Use Case |
|----------|------|----------|
| `-T0` | Paranoid | IDS evasion — extremely slow, one probe every 5 min. Rarely used practically. |
| `-T1` | Sneaky | IDS evasion — slow, minimal footprint. Used for stealthy long-term recon. |
| `-T2` | Polite | Slows down to use less bandwidth/target resources. Good for sensitive/legacy systems. |
| `-T3` | Normal | Default. Balanced speed, no special timing adjustments. |
| `-T4` | Aggressive | Faster scans, assumes a reliable, fast network. Most common choice in labs/CTFs and internal pentests. |
| `-T5` | Insane | Very fast, sacrifices accuracy — may miss results on slow/unstable networks. Used when speed is critical and false negatives are acceptable. |

### What's Actually Happening Under the Hood

Timing templates aren't just "speed presets" — they adjust several underlying parameters:

1. `--min-rtt-timeout` / `--max-rtt-timeout` / `--initial-rtt-timeout` — how long Nmap waits for a probe response before retrying or marking it as filtered.
2. `--max-retries` — how many times a probe is resent if no response.
3. `--scan-delay` / `--max-scan-delay` — minimum time between probes sent to the same host.
4. **Parallelism** — how many probes are sent simultaneously (`--min-parallelism` / `--max-parallelism`).
5. **Host timeout** — how long before Nmap gives up on an unresponsive host entirely.

Higher templates (T4/T5) shrink timeouts and increase parallelism = faster but noisier and more likely to trigger IDS/IPS or overwhelm rate-limited targets. Lower templates (T0/T1) do the opposite = slow but far less detectable.

### Professional/Real-World Guidance (10 yrs experience talking)

- **Internal pentest / lab / CTF:** `-T4` is my default — fast, reliable on modern LANs.
- **External engagement against a client's production infra:** I usually stick closer to `-T3`, sometimes `-T2`, to avoid crashing fragile services or tripping alarms before I want to (timing matters in red team engagements where stealth = objective).
- **IDS/IPS evasion assessments:** `-T0`/`-T1` combined with fragmented packets (`-f`), decoys (`-D`), and randomized scan order — but this is a specialized use case, not everyday scanning.
- **Never use `-T5` on production systems** — you WILL get inaccurate results and potentially cause instability on rate-limited or older devices.

---

## 2. Practicing

- IP random IP discover from Shodan website
- `nmap -O 41.216.188.185` → ip scan
- `nmap -O -T3 41.216.188.185` → ip scan using time templates
- `nmap -O -T3 41.216.188.185 -Pn` → ip scan
- `nmap -sV --version-all -T4 108.160.142.159`
- `nmap --host-timeout 300s 108.160.142.159` → timeout for time to stop scanning
- `nmap --min-parallelism 100 108.160.142.159` → parallel scanning for 100
- `nmap --min-hostgroup 30 108.160.142.1/24`
- `nmap --initial-rt-timeout 5000ms 108.160.142.159`
- `nmap --max-retries 3 108.160.142.159`
- `nmap -sV -T4 --max-retries 3 108.160.142.159`
- `nmap --ttl 50 108.160.142.159`
- `nmap --host-timeout 1m 192.168.44.1`
- `nmap -O -sV -A -T4 --scan-delay 7s 108.160.142.152` → scan delay cmd
- `nmap --min-rate 30 192.168.44.2` → packets deciding
- `nmap --defeat-rst-ratelimit 192.168.44.`

---

## 3. Firewall & IDS Evasion with Nmap

Notes and practice commands on detecting and evading firewalls/IDS during scans — for authorized lab environments only.

### 🎯 Objective

Understand how firewalls affect scan results and learn techniques to reduce detection footprint during reconnaissance.

### 🔎 How Firewalls Affect Nmap Results

| State | Meaning |
|-------|---------|
| **Open** | Port accepting connections |
| **Closed** | Port reachable, no service, RST received |
| **Filtered** | No response / dropped by firewall — Nmap can't tell if open or closed |
| **Unfiltered** | Port reachable but state can't be determined (common with ACK scans) |

### 🧪 Detecting a Firewall

```bash
# ACK scan to map firewall rulesets
nmap -sA target

# Compare with SYN scan results
nmap -sS target
```

If ACK scan shows "unfiltered" but SYN scan shows "filtered" → stateful firewall likely present.

### 🥷 Evasion Techniques

**1. Fragment packets** — splits TCP header across packets to bypass simple packet filters
```bash
nmap -f target
nmap -ff target   # smaller fragments
```

**2. Decoy scanning** — hides your real IP among fake ones in logs
```bash
nmap -D RND:10 target
```

**3. Source port manipulation** — spoof scans as coming from trusted ports (e.g. DNS 53, HTTP 80)
```bash
nmap --source-port 53 target
```

**4. Slow timing** — reduces chance of triggering rate-based IDS alerts
```bash
nmap -T1 target
```

**5. MTU/packet size manipulation**
```bash
nmap --mtu 24 target
```

**6. Randomize scan order / target order**
```bash
nmap --randomize-hosts target
```

**7. Spoof MAC address** (local network only)
```bash
nmap --spoof-mac 0 target
```

### ⚠️ Important Notes

- Evasion techniques are for **authorized red team/pentest engagements** and lab practice only.
- Modern IDS/IPS (Snort, Suricata, enterprise firewalls) often detect fragmentation and decoy scans easily — don't rely on these as guaranteed bypass methods.
- Always combine evasion with **timing templates** (`-T0`/`-T1`) for realistic stealth scenarios.

### 📸 Practice Evidence

- `scan-output-syn.txt`
- `scan-output-ack.txt`
- `scan-output-fragmented.txt`

### 📚 Resources

- [Nmap Firewall/IDS Evasion Docs](https://nmap.org/book/man-bypass-firewalls-ids.html)

---

## 4. Practicing

- `nmap -f 5.189.221.107` - fragment packets (small packets sends to take info)
- `nmap -f --send-eth 51.159.115.115`
- `nmap --mtu 16 51.159.115.115`
- `nmap -D decoy 1ip, 2ip, 3ip`
- `nmap -sI 192.168.0.2 51.159.115.115` → zombie hunt kill 2nd ip details
- `nmap --source-port 53 sheryians.com`
- `nmap -g 20 sheryians.com`
- `nmap --data-length 25 51.159.115.115 -Pn`
- `nmap --randomize-host 192.168.44.254`
- `nmap --spoof-mac 0 192.168.44.1`
- `nmap --badsum 192.168.44.254`

- `nmap -oN scan.txt --badsum 192.168.44.254` → result in file
- `nmap -append-output -oN --spoof-mac 0 192.168.44.1` → in file
- `nmap -oX scan.xml -sV --badsum 192.168.44.254`
- `cat scan.xml`
- `nmap -oG scan1.txt -A 192.168.44.254`
- `nmap --stats-every 2s -A 192.168.44.254`
- `nmap --stats-every 2m -A 192.168.44.254`
- `nmap -h` → details
- `man nmap`
- `nmap --reason`
- `nmap --pracket-trace`

---

## 5. Nmap Scripting Engine (NSE)

NSE is a powerful feature of Nmap that allows users to write and execute Lua scripts to automate a wide range of networking tasks beyond basic port scanning — including vulnerability detection, advanced service enumeration, backdoor/malware detection, and even exploitation. It essentially turns Nmap from a simple scanner into a flexible security assessment framework.

### How It Works (short)

Once Nmap discovers open ports and services, NSE scripts run against those services to gather deeper information or test for specific conditions — interacting with the actual protocol (HTTP, SMB, FTP, SSH, etc.) rather than just checking if a port is open.

### Script Categories

| Category | Purpose |
|----------|---------|
| `auth` | Bypass or test authentication mechanisms |
| `broadcast` | Discover hosts via broadcast/multicast queries |
| `brute` | Brute-force credentials (FTP, SSH, HTTP, etc.) |
| `default` | Safe, commonly useful scripts run with `-sC` or `-A` |
| `discovery` | Deeper enumeration of services/networks |
| `dos` | Test for denial-of-service vulnerabilities (use with caution) |
| `exploit` | Actively exploit known vulnerabilities |
| `external` | Scripts that query external resources (e.g. WHOIS) |
| `fuzzer` | Send unexpected/malformed data to find bugs |
| `intrusive` | May crash services or trigger alerts — not "safe" |
| `malware` | Detect backdoors/malware infections |
| `safe` | Won't crash services, safe to run broadly |
| `version` | Assists with service/version detection |
| `vuln` | Check for specific known vulnerabilities (e.g. CVEs) |

### Common Usage

```bash
# Run default safe scripts
nmap -sC target

# Run a specific script
nmap --script=http-title target

# Run scripts by category
nmap --script=vuln target

# Combine with aggressive scan
nmap -A target
```

Scripts are located at: `/usr/share/nmap/scripts/`
and can be updated via: `sudo nmap --script-updatedb`

### Professional Note (10-yr perspective)

`--script=vuln` is one of the most valuable categories for real assessments — it flags known CVEs on detected services automatically. But treat it as a **lead generator, not proof** — always manually verify any vulnerability NSE flags before reporting it to a client; false positives happen, especially with version-based detection.

### Practicing

- `nmap --script "smtp*" sheryians.com`
- `nmap --script whois-ip.nse,http-proxy-brute.nse 87.106.151.10`
- `nmap --script-args sheryians.com`
- `nmap --script rlogin-brute --script-args rlogin-brute.timeout=20s sheryians.som` → in root
- `nmap --script descovery 87.106.151.10`
- `nmap --script intrus\ive 83.223.203.116`
- `nmap --script auth sheryians.com`
- `nmap --script malware 130.64.213.206`
- `nmap --script external sheryians.com`
- `nmap --script malware --script-trace 130.64.213.206`

**Shodan API usage:** (after login to Shodan)
- `nmap --script shodai-api --script-args shodan-api.apikey=*****`
- `ndiff filename filename` → compare 2 reports