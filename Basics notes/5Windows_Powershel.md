## Power shell Windows

# about it
    . camand line
    . We read data with using it 
    . we know current status of system by using its cmd
    
# about cmd 
    . one liner cmd
    . fast output 
    . manual

    # a diffrent thing Scripting
    . For automation
    . Repeatable task perfoing by using it

# Types of Powershell available in pc
    . ise ( scripting environment like vs code)
    . Powershell terminal cmd

# Why Powershell

    . Powershell lolbin(living off the land )
    . Deep system visibility
    . Low noise attack
    . Internal info gathering
    . automation

# Comands
    . Get-Process -> Virtual task manager
    . Get-Process | Sort-Object CPU -Descending -> all uses of cpu whre happing
    . Get-Process | Sort-Object anyapp -Descending
    .  Get-Process | Where-Object {$_.Path -like "*AppData*"} -> to find where something runningget info 
    .   Stop-Process -Id 4068 -Force -> to stop something that running
    . Get-Service -> running and stoped services
    .  Get-Service | Where-Object {$_.Status -eq "Running"} -> see services only running
    . Get-NetTCPConnection -> Get connections details
    . Get-NetTCPConnection -state listen -> shows only listening state 
    . netstat -> all ip address connected
    . netstst -ano -> all ip address
    . ipconfig -> connected networks Ip adress
    . ipconfig -all -> in more detail
    . Get-childitem -> all files 
    . Get-Childitem -hidden -> for hidden files
    . attrib +s +h filename -> to hide this file that dont show in tridistion ay
    . attrib -s -h filename -> to the hidden file
    . Get-Childitem -recurse -> files with folders and file in the folders show all
    . Get-Filehash filepath -> file details
    . PS C:\WINDOWS\system32> Get-itemproperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\run" -> to see running items apps
    .  netsh wlan show profiles _> all connected wifi connections past & now
    .  netsh wlan show profiles name:"shahil" key=clear -> see passwords of wifi
    . Get-MPcomputerstatus -> get status of computer
    . Get-MPScan -scanType Fullscan -> anti virus scan
    . Get-winevent -logname security -maxevents 20 -> see events
    . Start-sleep -seconds 3 -> to delay the task
    . Get-ExecutionPolicy
    . Get-Process | Export-Csv REPORT.CSV -NOTYPEinformation 
    . Test-Connection name -> see connection
    . Get-netIPaddress -> ip address
    . Get-NetAdapter -> addapter connections show
    . Get-LocalGroup -> local group / Users
    .Get-ScheduledTask | where-object{$_.state-eq"READY"} -> ready tasks
    . WhoamI -> your name show
    . Hostname
    . Get-ExecutionPolicy bypass-scopeProcess
    