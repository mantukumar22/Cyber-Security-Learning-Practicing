### VAPT : Part 2

## Scanning & Enumeration 
## Notes : [https://sheryians.notion.site/Vulnerability-Assessment-Part-2-Scanning-Enumeration-30a1267520c780e6a04fd07172a8b44c?source=copy_link]
## Practicng

1. Scanning ip's 
    - `Angry IP scanner app :- scan ip's in windows use with ip ranges with Netmask `

    - `arp-scan --localnet :- in linux for scaning local netwok ip's`
    - `netdiscover :- ip dicover local ip`
    - `ifconfig :- check your own ip`
    - `netdiscover -r 192.6810.1/32 `
    - `fping  :- `
    - `fping -a -g 192.168.10.1/24 2>/dev/null  :- only live ip's`
    - `fping -a -g 192.168.10.1/24 2>/dev/null > live.txt :- details in file `
    - `nmap -sn 192.168.10.211 : - checking host is up or not`
    - `nmap -sn -PS22,80,443 192.168.10.1/24`
    - `nmap -sL 192.168.10.211 `
    - `nmap -sU 192.168.247.129 -> udp scan `
    - `nmap -scanflags FIN 192.168.247.129 `
    - `nmap -scanflags ACK 192.168.247.129 `
    - `nmap -sA 192.168.247.129`
    - `nmap -sO 192.168.247.129`
    - `nmap --send-eth 192.168.247.129`

    service & version detection
    - `nmap -sV 192.168.247.129`
    - `nmap -O 192.168.247.129`
    - `nmap -A 192.168.247.129`
         /usr/share/nmap/scripts
    - `nmap --script vuln 192.168.247.129`
    - `nc 80 192.168.247.129 `
    - `nmap --script smb* vuln 192.168.247.129 `
    - `curl -l sheryians.com`
    - `nc -nv 192.168.247.211`
    - `openssl s_client -connect 192.168.247.129:443 --tls1_2`
    /Documents
    - `enum4linux 192.168.247.129 `
    - `enum4linux -A 192.168.247.129 `
    - `smbclient -L //129.168.247.129 -N `
    - `smbclient -L //129.168.247.129 -U guest `
    - ` rpcclient 192.168.247.129`
    - `nmap -p445 --script smb-enum-shares 192.168.247.129`
    - `nmap -p445 --script sub-enum* 192.168.247.129`
    - `snmpwalk -v2c -c public 192.168.247.129 `
    - `snmpwalk -v2c -c private 192.168.247.129 `
    - ` snmp-check 192.169.247.129 `
    - `subfinder -d sheryians.com`
    - ` dirbuster = sheryians.com -> tool`
    - ` whatweb 192.168.247.129`
