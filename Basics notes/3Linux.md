### Linux 
## Linux is a Dabians community where people build thier own opearating system(OS) open source.
    .Open_Source
    .Lighweight Infrastructure (4gb ram)
    .Shell/Command-line Interface

## History & Evolution
    .In 1970 Unix 
    .Then Linus Torvalds built Linux kernel in 19191
    .In world 3 OS (Mac, Windows, Linux)
    .(90% of server use linux)
    . cloud thing made on linux

## Linux Distros
    .Black Arch
    .kali Linux ( hacker frendly)
    .fedora
    .devian & Obantu

## Why Linux Matters
    .for cyber security- Tools, cmds, more
    .For DevOps - Automations, server hoasting
    .sysdmins 

## Linux File System Structure
    /       /var
    /bin    /dev
    /sbin   /proc   /mnt
    /etc    /tmp    /media
    /home   /opt    /srv
    /usr    /lib

## Linux Install
    . go to kali.org
    . view on website for more info ( where and what run)
    . I choose Virtual Machine in VMware workstation
        ( in install take too much time) install 
    . 253 tools free in Linux

## Open Linux in VM ware
    file system understand because you need to under stand where & which file present for doing quick moves on other systems 

    . explore and check file system folder
    .   /bin -> this is where all tools     related and work like exe file ( present all tools packages)
    by terminal use / cmd you enter the file system then use cd bin etc.
    . /boot -> boot related files
    . /dev
    . /etc -> issue resolve here
    . /lib -> library tools binary files
    . /lib32 -> same like program files
    . /media 
    . /opt -> installed extrnal files
    . /proc -> kernal files
    . /root
    . /logs 

## Initial Commands
    . pwd -> where you are in in folder
    . ls -> see lower dict from where you are
    . ls -la
    . cd -> change dict for enter in any folder
    . cd ../ -> back
    . tree -> tree view of folders & files
    . mkdir name -> make directory
    . rmdir name -> remove dir( file or folder is empty)
    . rm -rs name -> remove file or folder fourcefully
    . cp name path -> for copy
    . mv name path -> move folder or file
    . man name cmd -> details what cmd do
    . touch filename -> make file
    . touch content filename -> write in file
    . cat filename -> see what is in file
    . echo -> for print something on cmd
    . history -> see privous used commands
    . less foldernamewith filename -> everything in file see
    . head -> to what top of in file
    . tail -> to see bottom line in file
    . wc filename -> to words in file
    . clear -> to clear screen

    # system info cmd
    . uname -> to see what you are using ( Linux)
    . hostname -> (name) to what you write in hostname 
    . uptime -> time of system running
    . df -> see dic  with /
    . du 
    . ps -> for see process
    . top -> task manager
    . fdisk -> to partition of programs create & manipulate
    . lsblk -> see disk managment
    . lsusb -> connected usb

    # sign cmd

    . echo "something" > filename -> to write in file
    . echo "something" >> filename -> to overwrite in file  
    . tree < filename -> tree like stc
    . cat file name | grep text -> to find see a error or text word
    . awk -> searching passwords
    . sed 
    . tr -> to change a -> A everything in file
    . strings -> line by line files
    . ls | tee path -> see what present in path
    . find / word path -> search that word in all files
    . locate filename -> search file
    . which nametool -> to see where is that tool
    . whereis file -> full path see of where is file
    . diff 
    
    # create user in linux

    . useradd mantu
        if permission not 
        . sudo su (super user like adminstrator in pc)
    .userdel mantu -> to delete user  
    
    . create user with no permision
        .adduser name
        New password: set
        detail
    
    . passwd -> change password

    .cat /etc/shadow
    .sudo cat /etc/shadow -> all users and password in your running system in chrome user pswd etc.
    .cat /etc/shadow -> groups see
    . chmod 777 filename -> give permission all read write exicute 
    . chgrp filename -> change permission
    . pgrep
    . pkill -> see, stop, progess running process
    . nice

    # package managment

    . apt-get update -> make your system installing ready
    . dpkg -i filename.deb -> to install debian files
    
    # To install tools from github
    . git clone github repl url 

    . fdisk -help
    . fdisk -l -> see available disks

    # Networking 
    . ifconfig -> see every netwoks details
    . ifconfig -a -> mac address
    . io link -> Internet connection
    . ping google.com -> 
    . traceroute google.com -> details of going detail and more from my pc to google
    . mtr 
    . nslookup google.com -> to get details of company like ip addresh
    . dig google.com

    # system monitor ( need to install )
    . htop
    . iotop

    . sar
    . dseg
    
    . sudo apt --fix-brokrn install -> to fix everything broken except Your heart
    . sudo dpkg --configure -a -> make broken issues




