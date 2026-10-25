To pass the OSCP,  need both theory and practice. PG Practice is a way to gain practical experience.
In the beginning, even rooting easy boxes was very difficult for me. So, I got help from AI, read other people's write-ups, analysed the boxes, updated my notes on commands, tools, and concepts, and practised those concepts.
By the end, I felt somewhat confident in using my tools and applying what I had learned.
I'm not publishing my Linux box notes because there are many things to correct in the earlier boxes, and I didn't document the later boxes as much since they didn't introduce many new concepts.
However, there are only a few Windows and AD boxes, so I've briefly documented my notes here. If someone needs a clue or wants to practise a specific skill or attack, they can use these notes to find a relevant box.

# AD
### Access
- Use `.htaccess` to bypass upload restrictions
- Use Rebus without credentials for Kerberoasting
- Exploit `SeManageVolumePrivilege` → manual DLL hijacking
### Heist
- SSRF and Responder to capture NTLMv2 hashes → crack passwords
- Abuse `ReadGMSAPassword`
- Abuse `SeRestorePrivilege` with Utilman via RDP
### Vault
- Windows library file → capture NTLMv2 hash
- GPO abuse via pyGPOAbuse or SharpHound
### Nagoya
- Username enumeration via website team names using Kerbrute
- `strings -e l` binary analysis → hardcoded credentials
- Kerberoasting for MSSQL
- MSSQL silver ticket → export the ticket → Impacket MSSQL → reverse shell
- `SeImpersonatePrivilege` escalation
### Resourced
- Credentials in user descriptions
- Credential dumping from `ntds.dit`
- `SeMachineAccountPrivilege` + Resource-Based Constrained Delegation
### Hutch
- Use `ldapsearch` to retrieve user descriptions containing credentials
- BloodHound reveals LAPS read permission → [pyLAPS](https://github.com/p0dalirius/pyLAPS) to dump credentials
## Windows

### Algernon
- No version disclosure → identify potential exploits using port numbers

### Authby
- Anonymous FTP enumeration reveals Offsec and admin users
- `admin:admin` credentials work on the FTP server
- FTP contains `.htaccess` (Offsec hash) and the web root
- Upload `shell.php` to FTP → log in to the web application using Offsec credentials → trigger the shell
- `SeImpersonatePrivilege` abuse for privilege escalation

### Craft
- Initial access via a malicious macro in a CV uploaded to the web application → executed by the user process → shell
- Since files can be added as the Apache user, upload `shell.php` and trigger it via the web server for a reverse shell
- `SeImpersonatePrivilege` abuse

### Internal
- Nmap reveals an SMB CVE → shell

### Jacko
- Identify version → RCE CVE
- RCE is tricky → run commands using full paths
- `SeImpersonatePrivilege` abuse

### Kevin
- Identify version → buffer overflow RCE
- Modify the exploit by adding a generated payload from `msfconsole`

### Squid
- Identify Squid proxy → use HackTricks notes for Squid proxy service penetration testing
- Access internal XAMPP through the proxy
- Use phpMyAdmin (default credentials) and SQL queries to write `shell.php` → RCE
- PHP code to verify whether the upload succeeded
- Useful when the payload size is limited

### DVR4
- Identify version and CVE for directory traversal
- Steal SSH `id_rsa` → SSH access
- CVE for weak password encryption → decrypt hardcoded password
- Exploit fails to guess the final character → brute-force it

### Hept
- Identify unknown ports 79 and 110 → use HackTricks for enumeration
- Guess mail server credentials using web information: `jonas:SicMundusCreatusEst`
- Client-side attacks using LibreOffice files
- Binary hijacking of an auto-starting service
- Reboot

### Shenzi
- Guess web root using the box name
- Use credentials found on FTP for web admin access
- WordPress reverse shell via template editor
- WinPEAS reveals `AlwaysInstallElevated`
- This means `.msi` files (Microsoft Software Installer packages) are automatically installed with administrative privileges
- Privilege escalation → SYSTEM

### Nickle
- Web application IP error reveals other ports and endpoints
- POST request to an endpoint reveals running processes and credentials → decode encoded passwords
- SSH access using those credentials
- Find encrypted PDF → decrypt it → PDF reveals an endpoint for executing commands as SYSTEM

### Slort
- PHP web application has an RFI vulnerability
- Use it for RCE
- Replace scheduled task binary with a trojan

### Medjed
- Unusual box → default credentials allow file access on disk → retrieve flags

### Monster
- Gobuster reveals the `blog` endpoint, which discloses the version
- CVE provides authenticated RCE
- Another endpoint reveals users
- Guess password → second name of the user
- Installed XAMPP version: 7.3.10
- Local privilege escalation CVE exploit available
