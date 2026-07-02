### Nmap :-----

 Notes of sheriyans :-- [https://sheryians.notion.site/Nmap-Tutorial-Part-1-2fd1267520c78034aaf7d327eed8d53e]
1. Check :- https://nmap.org/download#windows
    In kali linux: use nmap
    In Windows: Zenmap 

    Nmap (Network Mapper) — Overview & Working
    What it is:
    Nmap is an open-source network scanning tool used to discover hosts, open ports, running services, 
    service versions, and OS fingerprints on a network. In a security context, it's the go-to tool 
    for the reconnaissance and enumeration phase of a penetration test or vulnerability assessment.
    
    How it works (short):
    1. Host Discovery — Nmap first checks which hosts are "alive" using [ICMP echo], 
    [ARP] requests (on local networks), or TCP/UDP probes to common ports. 
    This avoids wasting time scanning dead IPs.
    2. Port Scanning — It sends specially crafted packets (TCP SYN, TCP Connect, UDP, etc.) 
    to target ports and analyzes the responses:
        . Open — service responds (SYN-ACK received)
        . Closed — port reachable but no service (RST received)
        . Filtered — no response / blocked by firewall


    3. Service/Version Detection (-sV) — Once open ports are found, Nmap sends protocol-specific 
    probes and compares responses against its service-probe database to identify the software 
    and version running (e.g., Apache 2.4.41).
    4. OS Detection (-O) — Analyzes subtle differences in how a target's TCP/IP stack responds 
    crafted packets (TTL, window size, etc.) to guess the OS.
    5. NSE (Nmap Scripting Engine) — Lua-based scripts (--script) that extend Nmap 
    for vulnerability detection, brute-forcing, malware detection, and deeper enumeration.

    In one line: Nmap sends crafted packets to a target, interprets the responses, and builds a map of 
    what's alive, what's open, and what's running — forming the foundation for every next step in a security assessment.

2. Camands :---
    . man namp -> to see what is nmap in terminal

    Work : [scan and check ip up or down , path to go to url , host up, more ]
    . find any id on shodan
    . nmap google.com -> see Host , Port, state, service & more about url target
    . nmap ip -> same
    . nmap google.com facebook.com -> same
    . nmap 192.168.44.1-55 -> ip range 1-55
    . nmap -il folderpath 
    . nmap -iR 12
    . nmap 192.168.44.155 -exclude 192.168.44.1
    . nmap -A 183.60.25.*** -> shows opearating system
    . nmap -A 183.60.25.***  -Pn
    . nmap -Pn ip -> shows something related to firewall
    . nmap -sn ip -> show target up or down
    . nmap -PS80 ip
    . nmap -PU80 ip
    . nmap -PO80 ip
    . nmap -PA443 google.com -> more info ips and hots up
    . nmap -PA443 google.com -Pn
    . nmap -PY google.com
    . nmap -PY google.com -Pn
    . nmap -PE google.com
    . nmap -PE google.com -Pn
    . nmap -PP google.com
    . nmap -PR google.com
    . nmap --traceroute google.com -> shows how many paths to go google from me
    . nmap -R 170.66.0.**
    . nmap -PR -R 170.66.0.**
    . nmap -n 170.66.0.**
    . nmap --system-dns 192.168.44.25*
    . nmap -sl ip


    Connect How - TCP scan connect 
    . nmap -sT 109.70.208.*** -> connect with port and tell how many connected port with this port
    . nmap -sT 109.70.208.*** -Pn
    . nmap -sT google.com
    . nmap -sU google.com
    . nmap -sN 109.70.208.*** 
    . nmap -sX 109.70.208.***
    . nmap -sA 109.70.208.*** 
    . nmap -sO 109.70.208.***
    . nmap -scanflags FIN 109.70.208.***
    . nmap -scanflags URG 109.70.208.***
    . nmap -scanflags PSH 109.70.208.***
    . nmap --send-eth 109.70.208.***
    . nmap --send-ip 109.70.208.***


    Services & Version Scan
        fast scan , etc
    . nmap -F 156.240.28.***
    . nmap -p 22,23 156.240.28.***
    . ssh 156.240.28.2**
    . nmap -p 1-100 156.240.28.2**
    . nmap -p 1-100 156.240.28.2** -Pn
    . nmap -p ssh 156.240.28.2** 
    . nmap -p ssh 156.240.28.2** -Pn
    . nmap -p U:443,T:22 156.240.28.2** 
    . nmap -p ftp ip
    . nmap --top-ports 30 64.32.82.6
    . nmap --top-ports 30 64.32.82.6 -Pn
    . nmap -r 64.32.82.6

    . nmap -sV 64.32.28.24
    . nmap -sV --version-intensity 0-9 64.32.28.24 -> intensity range 0-9
    . nmap -sV --version-intensity 9 64.32.28.24 -> intensity high 9
    . nmap --version-all 64.32.28.24
    . nmap -sV --version-light 64.32.28.24
    . nmap --version-trace 0-9 64.32.28.24 
    . nmap -sR 185.47.42.86
    . nmap -sS -sV 185.47.42.86 -Pn
    . nmap -O 185.47.42.86
    . nmap -O 185.47.42.86 -v
    . nmap -O 185.47.42.86 -v -Pn
    . nmap -O --osscan-limit 185.47.42.86
    . nmap -O --osscan-guess 185.47.42.86
    . nmap -O --fuzzy185.47.42.86
    . nmap -sS -O 185.47.42.86
    . nmap -O --packet-trace 185.47.42.86
    . nmap -O -sV -A --reason 185.47.42.86