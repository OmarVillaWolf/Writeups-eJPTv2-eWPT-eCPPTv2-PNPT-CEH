# User Hunting 

Tags: #AD #Powershell #PowerView 

* [Invoke-SessionHunter](https://github.com/Leo4j/Invoke-SessionHunter)

```powershell 
❯ Find-LocalAdminAccess -Verbose
❯ Find-DomainUserLocation -Verbose
❯ Find-DomainUserLocation -UserGroupIdentity "RDPUsers"
❯ Find-DomainUserLocation -CheckAccess
❯ Find-DomainUserLocation -Stealth 

❯ Invoke-SessionHunter -FailSafe
❯ Invoke-SessionHunter -NoPortScan -Targets C:\AD\Tools\servers.txt
```