## AD
### Access  
- use .htaccess for bypass upload restriction 
- use Rubeus without credentials for kerberoasting 
- Exploit SeManageVolumePrivilege --> manual DLL hijacking
### Heist
- SSRF and responder capture ntlmv2 --> crack password
- abuse ReadGMSAPassword 
- SeRestorePrivilege abuse with utilman call via RDP
### Vault
- Windows library file --> crack ntlmv2
- GPO abuse via pyGPOAbuse or SharpHound

### OSCP A,B,C
- Credential dumping via mimikatz
- Credential dumping from windows.old
- SeImpersonatePrivilege --> PrintSpoofer
- Kerberoasting --> mssql --> shell
- Binary string analysis --> hardcoded creds
- PowerShell command history creds

### Nagoya
- Userenum via website team names using kerbrute
- strings -e l binary analysis --> hardcoded credentials
- Kerberoasting for mssql
- mssql silver ticket --> export the ticket --> impacket mssql --> rev shell
- SeImpersonatePrivilege escalation

### Resourced
- Credentials on user description
- Credentials dump from ntds.dit
- SeMachineAccountPrivilege + Resource-Based Constrained Delegation
### Hutch
- Use ldapsearch to return user descriptions that have credentials
- BloodHound shows LAPSread permission --> [pyLAPS](https://github.com/p0dalirius/pyLAPS) to dump credentials
