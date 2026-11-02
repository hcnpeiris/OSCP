## Ports Scan
- One command
```bash
nmap -p- -Pn $IP  -v --min-rate 1000 --max-rtt-timeout 1000ms --max-retries 5 -oN nmap_ports.txt && sleep 5 && nmap -Pn $IP -sV -sC -v -oN nmap_sVsC.txt && sleep 5 && nmap -T5 -Pn $IP -v --script vuln -oN nmap_vuln.txt
```
- Find open ports
```bash
nmap $IP -p- -oN /tmp/ports.txt && cat /tmp/ports.txt | grep -E '^[0-9]|^$' | awk '{print $1}' | sed ':a;N;$!ba;s|/tcp\n|,|g' > port.txt
```
- Scan opened ports
```bash
nmap $IP -vvv -A -p$(sed 's/,$//' port.txt) -oN nmap.txt
```
- Scan udp ports
```bash
sudo nmap -Pn -n $IP -sU -p- --min-rate 1000 --max-retries 1 --open
```
- `-sU -sS` can use for both UDP and TCP
- try `udpx`
## SNMP
- Must do UDP port scan for SNMP
```
echo -e "public\nprivate\nmanager" > communities.txt  
onesixtyone -c communities.txt $ip
```
- if it shows public
```
snmpwalk -c public -v1 $ip 1.3.6.1.2.1
```
- 
```
snmpwalk -c public -v1 $ip .1.3.6.1.4.1.77.1.2.25   # Users  
snmpwalk -c public -v1 $ip .1.3.6.1.4.1.77.1.2.3.1.1 # Processes
```
## SMB
- Anonymous list shares
```bash
smbclient -N -L //$IP
```
- Share with credentials
```bash
smbclient -L //$IP -U "$eric%$ericpasswd"
```
- Specific share
```bash
smbclient //$IP/NETLOGON -U 'fiona.clark%Summer2023'
```
- List all files and dir with smbclient
```bash
recurse ON
ls
```
- Download multiple files with smbclient
```bash
prompt OFF
mget *
```
- [NetExec SMB Documentation](https://www.netexec.wiki/smb-protocol/authentication/checking-credentials-domain)
### SMBMap
- File permissions
```bash
smbmap -H $IP  #with credentials -u fiona.clark -p Summer2023
```
- List all files and dir with smbmap using `-r`

## FTP
- FTP login - Then enter username, credentials
```bash
ftp $IP
```
- For anonymous - `anonymous:anypasswd`
- `ls -a` for hidden files
- Download whole ftp
```bash
wget -r ftp://Anonymous:pass@$IP
```
- `lftp` - supports FTPS, SFTP, HTTP/HTTPS, and FISH.
## Web
### Gobuster
- basic file enumeration
```
gobuster dir -u http://$IP -w /usr/share/seclists/Discovery/Web-Content/common.txt -r
```
- `-x` for Extensions
- Sites that do not have 404 instead use 302. You need to use `-b 404,302` to filter both  
- Or use `--exclude-length value` filter based on response size. 
- Extended enumeration
```
gobuster dir -u http://$IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
- `-H 'Header2: val2'` to add headers 
- Alternatives fuzz, dirbuster
### WPScan
- Enumeration
```
wpscan --url http://IP --enumerate ap,at --plugins-detection aggressive
```
### Nuclei
- Basic scan
```
nuclei -u http://$IP
```

- Password attacks
```
wpscan --no-banner --url http://$IP --usernames hugo,c0ldd,philip --passwords /usr/share/wordlists/rockyou.txt
```

## Random port
- Try Hacktricks - https://hacktricks.wiki/en/network-services-pentesting/pentesting-ftp/index.html
```bash
nc -zv <host> <port>
```
## Common ports
- **80** - HTTP
- **22** - SSH
- **139** - NetBIOS/SMB
- **135** - MSRPC 
- **445** - SMB
- **21** - FTP
- **3306** - MySQL / MariaDB
- **8080** - HTTP / Tomcat / web services
- **53** - DNS
- **3389** - RDP
- **443** - HTTPS
- **5985/5986** - WinRM
- **1433** - MSSQL
- **5432** - PostgreSQL 
- **88** - Kerberos
- **389** - LDAP
- **464** - Kerberos password change
- **593** - RPC over HTTP
