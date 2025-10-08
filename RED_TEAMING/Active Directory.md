
#### The following is a list of Active Directory components:
```
Organizational Units (OU's): are containers within the AD domain with a hierarchical structure.

Active Directory Objects: can be a single user or a group, or a hardware component, such as a computer or printer. Each domain holds a database that contains object identity information that creates an AD environment, including:

    1.Users - A security principal that is allowed to authenticate to machines in the domain
    2.Computers - A special type of user accounts
    3.GPOs - Collections of policies that are applied to other AD objects

AD domains: are a collection of Microsoft components within an AD network. 

AD Forest: is a collection of domains that trust each other. 
```

#### The following are Active Directory Administrators accounts:
```
BUILTIN\Administrator: Local admin access on a domain controller
Domain Admins: Administrative access to all resources in the domain
Enterprise Admins: Available only in the forest root
Schema Admins: Capable of modifying domain/forest; useful for red teamers
Server Operators: Can manage domain servers
Account Operators: Can manage users that are not in privileged groups
```

STEPS:
1. Account discovery
2. User/Groups
3. Enumerate antivirus and security detection
4. Undetectable (Don't generate any suspicious log)

##### It is a set of software applications used to monitor and detect abnormal and malicious activities within the host, including:
```
    1. Antivirus software
    2. Microsoft Windows Defender
    3. Host-based Firewall
    4. Security Event Logging and Monitoring 
    5. Host-based Intrusion Detection System (HIDS)/ Host-based Intrusion Prevention System (HIPS)
    6. Endpoint Detection and Response (EDR)
```


###### Command for find a computer is a part of AD environment or not
` systeminfo | findstr Domain `


###### The following PowerShell command is to get all active directory user accounts
```
Get-ADUser  -Filter * 
```


###### Using the Search-Base option, we specify a specific Common-Name CN in the active directory. For example, we can specify to list any user(s) that part of Users.
```
Get-ADUser -Filter * -SearchBase "CN=Users,DC=THMREDTEAM,DC=COM"
```


###### Checking for antivirus software
```
Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntivirusProduct
```
Windows servers may not have `SecurityCenter2` namespace, it works for Windows workstations! (windows 10/11...)


###### We can use the following PowerShell command to check the service state of Windows Defender:
```
Get-Service WinDefend
```


###### we can start using the `Get-MpComputerStatus` cmdlet to get the current Windows Defender status. However, it provides the current status of security solution elements, including Anti-Spyware, Antivirus, LoavProtection, Real-time protection, etc. We can use `select` to specify what we need for as follows
```
Get-MpComputerStatus | select RealTimeProtectionEnabled
```
As a result, `MpComputerStatus` highlights whether Windows Defender is enabled or not.


###### Check firewall profile using the `Set-NetFirewallProfile` cmdlet.
```
Get-NetFirewallProfile | Format-Table Name, Enabled
```


###### disable one or more than one firewall profile using the `Set-NetFirewallProfile` cmdlet.
```
Set-NetFirewallProfile -Profile Domain, Public, Private -Enabled False
```


###### We can also learn and check the current Firewall rules, whether allowing or denying by the firewall.
```
Get-NetFirewallRule | select DisplayName, Enabled, Description
```


###### During the red team engagement, we have no clue what the firewall blocks. However, we can take advantage of some PowerShell cmdlets such as `Test-NetConnection` and `TcpClient`. Assume we know that a firewall is in place, and we need to test inbound connection without extra tools, then we can do the following: 
```
Test-NetConnection -ComputerName 127.0.0.1 -Port 80
```
As a result, we can confirm the inbound connection on port 80 is open and allowed in the firewall. Note that we can also test for remote targets in the same network or domain names by specifying in the `-ComputerName` argument for the `Test-NetConnection`. 


###### We can get a list of available event logs on the local machine using the `Get-EventLog` cmdlet.
```
Get-EventLog -List
```


###### We can look for a process or service that has been named `Sysmon` within the current process or services as follows,
```
Get-Process | Where-Object { $_.ProcessName -eq "Sysmon" }
```
`sysmon` is a log collecting tool for a blue teamer so don't do anything silly which can make you detectable 
###### or look for services as follows,
```
Get-CimInstance win32_service -Filter "Description = 'System Monitor service'"
```
###### It also can be done by checking the Windows registry 
```
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\Microsoft-Windows-Sysmon/Operational
```
All these commands confirm if the `sysmon` tool is installed. Once we detect it, we can try to find the `sysmon` configuration file if we have readable permission to understand what system administrators are monitoring.
```
findstr /si '<ProcessCreate onmatch="exclude">' C:\tools\*
```



The following are some of the internal services that are commonly used that we are interested in:
```
DNS Services
Email Services
Network File Share
Web application
Database service
```



###### listing the running services using the Windows command prompt `net start` to check if there are any interesting running services.
```
net start
```
We can see a service with the name THM Demo which we want to know more about.

Now let's look for the exact service name, which we need to find more information.
```
wmic service where "name like 'THM Demo'" get Name,PathName
```

We find the file name and its path; now let's find more details using the Get-Process cmdlet. 
```
Get-Process -Name thm-demo
```
Once we find its process ID, let's check if providing a network service by listing the listening ports within the system.
```
netstat -noa |findstr "LISTENING" |findstr "3212"
```



###### Reset password
```
Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
```


