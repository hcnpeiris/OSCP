
### Algernon
- No version reveal but verified exploits via ports numbers
### Authby
- ftp anonymous enum reveal offsec, admin users
- admin:admin cred work on ftp server
- ftp have .htaccess (offsec hash) and the web root
- shell.php upload to ftp login using offsec cred to web and trigger the shell
- SeImpersonatePrivilege abuse for escalation
### Craft
- Initial access via crafting malicious macro CV and upload it to web, user process --> get the shell
- since files can be added to the Apache user, add shell.php and trigger via web for reverse shell
- SeImpersonatePrivilege
### Internal
- Nmap reveal smb CVE --> shell

### Jacko
- Find Version --> RCE CVE
- RCE is very tricky, you have to run commands with the full path
- SeImpersonatePrivilege
### Kevin
- Find Version --> buffer overflow RCE
- Exploit needed to modify via adding generated payload from msfconsole

### Squid
- find squid proxy and use HackTricks note for pentest squid proxy service
- using the proxy access internal XAMPP
- using phpMyAdmin (default creds) use sql queries for write shell.php --> RCE
- It has some PHP code for verify upload correctly happened or not
- Useful since payload size is limited

### DVR4
- Find version and CVE for traversal
- Steal the ssh id_rsa --> get ssh access
- Since there is CVE for weak password encryption --> hardcoded password decrypt
- exploit failed to guess final character --> brute force it
### Hept
- Find unknown ports 79 and 110 --> use HackTricks for enum
- 110 mail server cred guess by web information "jonas:SicMundusCreatusEst"
- Client side attacks using open libre files
- Binary hijacking of auto starting service
- Reboot

### Shenzi
- Web root guess by box name
- Use creds found on FTP for web admin
- WordPress rev shell via template editor
- WinPEAS reveal `AlwaysInstalledElevated` 
- This means that .msi files (_Microsoft Software Installer_) are automatically installed with Administrative privileges.
- Escalation this gets system

### Nickle
- web app ip error issue reveal other ports endpoints
- on endpoint with POST request reveal running process with reveal credentials --> decode encoded passwords
- SSH using those creds
- Found encrypted PDF --> decrypt it --> PDF reveal endpoint for exec commands as system

### Slort
- PHP web app has RFI vulnerability
- Use it for RCE
- Scheduled task binary replace to trojan

###  Medjed
- Very awkward box, just default creds can look at files on disk so I got the flags.

### Monster
- gobuster reveal `blog` endpoint and it mentioned the version
- CVE had an authenticated RCE
- users revealed by another endpoint
- guess password. It's the second name of the user
- installed XAMPP version 7.3.10.
- It has a local privilege escalation CVE exploit
