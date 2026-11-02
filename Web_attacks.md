## File Upload
- Bypass restriction via .htaccess - [Access](https://medium.com/@Dpsypher/proving-grounds-practice-access-b95d3146cfe9)
```bash
echo "AddType application/x-httpd-php .dork" > .htaccess
```
- 
## API
Rewrite data: User approval, Change another user's passwords or IDOR
Leak sensitive data: IDOR to leak high privilege user data
## XSS
Can use for priv escalation
## Path Traversal
Via ?page=../../../../../etc/passwd like parameter
Server traversal issue like encoded - %2f..%2f..

- SSH `id_rsa` steal
- Sensitive file read
- Plain text credentials steal
- Read config files
## File Inclusion
Execute local or remote file using traversal payloads
- Write php shell into log file and via LFI execute
- Trigger kali hosted php shells
## PHP wrappers
Need to learn more

**There are more, check the material**





## Old
- via file uploads, `authorized_keys` --> [[SSH]]
## Sensitive Information leak
- Path traversal
- SQLi
- Hardcoded credentials
- SSRF - http://, file://, ftp://
## Post exploitation
- Interactive shells
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
