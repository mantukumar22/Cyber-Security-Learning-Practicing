### VAPT - Practiceing And notes

# 1st Thing to do when Start

1. Browser based Recon
    . Domain - subdomains
    . ip
    . os
    . services
    . emplooyees
    . address
    . Tech stack
    . files


    Google Dorking -> Search on Google & any other browser about the company or org. ( Like:facebook, Iit Kanpur ).Using Advance google search

    Use these inBrowser :- 
    Github Repo for learn : [https://gist.github.com/sundowndev/283efaddbcf896ab405488330d1bbc06]
    site:gov -> shows only gov sites
    inurl:admin -> shows only admin panel of all websites
    intitle:login -> shows login 
    site:gov filetype:pdf -> pdf opend in gov websites
    ext:sql -> show sql files
    intext:password
    index of pdf books -> index files books on browser
    site:edu inurl:admin intile:login
    site:.in intitle
    site:.edu ext:sql OR ex.db
    site:.edu filetype.config OR filetype.cfg
    site:.in inurl:test OR inurl:staging
    site:. intext:"username" intext:"password"

    Chrome Extentions :-
    . netcraft -> all detailed info about company that matters
    . wappalyzer -> shows Tech stack 
    . S3 BucketList
    . x.com -> ip , open ports , hostname , details
    . Cookie-Editor
    . Wayback Machine -> old data & website
    . shodan ai -> past exposed details
    . hunter io 
    . Dotgit 
    . Gitleaks -> finding secrets and more  [https://github.com/gitleaks/gitleaks]
    . Gitrob
    . Security Hearders 
    Poodle scan
    . Google Hacking datbase
    . Heartbleed scan

2. DNS Recon Basics
    . In terminal run ali linux or In VM ware
    
    . host -t mx google.com -> shows all handles of google
    . dnsrecon -d facebook.com -> some infos
    . nslookup google.com -> Nmae & addresses
    . dig google.com 
    . tracert google.com -> routes of getting google from yor pc
    . dnsemun
    . masscan ip 
    . dirb urllink -> url directories
    

    tools
    . theHarvester  -> find Emplooyee details find 
        . theHarvester -d microsoft.com -b all

    . subfinder -> shows subdomains
        .subfinder -d google.com

    In Chrome 
    . signalHire -> Linked id detail 
     
    
    Subdomain With -> sublister & subfinder ans crt.sh weleakinfo

    . subfinder -d sheryians.com -> subdomains

    Challenges :-------
    [ Error: Failed to fetch http://http.kali.org/kali/pool/main/s/subfinder/subfinder_2.13.0-0kali1_amd64.deb  404  Not Found [IP: 54.39.128.230 80]
    Error: Unable to fetch some archives, maybe run apt update or try with --fix-missing? ]

    Then i do this [
        sudo apt update
        subfinder -d sheryians.com
    ]

    . sublister
        git clone https://github.com/aboul3la/Sublist3r.git
        ^ see this github then run commands

    . crt.sh -> in chrome
    . We leakinfo -> in Chrome
    . DeHashed -> in chrome for searching data leak info



    TCP :---
    . find ip by using shodan on browser
    .                                   port
    . nc -nvv -w 1 -z 199.180.255.*** 3388-3390

    . namp -sT 199.180.255.***
    . namp -sT -Pn 199.180.255.***

    UDP :---
    . namp -sU -Pn 199.180.255.***

    iptables :--
    . sudo iptables -l INPUT 1 -s Ip -j ACCEPT
    . sudo iptables -l OUTPUT 1 -d Ip -j ACCEPT

    . namp google.com
    . sudo iptables -vn -L

    . nmap -p 1-65535 IP
    
    os :--
    . nmap -O IP


    SMB Enumeration :--- Open things Like printers

    . find ip on shadan
    . nmap -p 139,445 --script smb-enum-shares smb-enum-users 72.49.139.96
    . nmap -p 139,445 --script smb-os-discovery smb-enum-users 72.49.139.96

    . smbclient -L https//:141.179.89.190 -N
    . crackmapexec smb :141.179.89.190
    . smbmap -H :141.179.89.190 -> see accseble or not 
    . 

    SMTP Enumeration : ----

    1. valid user & Invalid user
    2. exists user & Fake user

     . find ip on shadan
     . nmap -p25 --script smtp-enum-users 163.********8
     . nc ip
     
    SNMP : these thing i want :--- 

    1. hostname os
    2. network interface
    3. ip
    4. proces
    5. installed software

    . find ip on shadan
    . snmpwalk -v2c -cpublic 204.232.119.120 -> get info in snmp
    . nmap -sU -p161 --script snmp-info 204.232.119.120


3. Lab setup
    . Vulnlab :- chrome
    . open with vm ware
    . arp-scan -l in kali 