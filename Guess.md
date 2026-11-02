## Users
1. via website (team page, emails)
2. box name
3. Downloadable file metadata
4. Log files data (endpoint , errors)
5. sqli
6. dir brute force 
7. wpscan for wordpress
8. hidden messages

Extended lists
1. Website — team page, emails
2. Box name
3. Downloadable file metadata
4. Log files — endpoints, errors
5. SQL injection
6. Directory/file brute force → endpoint information
7. WPScan — WordPress usernames ([GitHub][1])
8. **WordPress author pages / author IDs**
9. **WordPress REST API**
10. **WordPress RSS feeds**
11. **Git repositories / exposed `.git`**
12. **Source-code comments**
13. **JavaScript files**
14. **Backup/config files**
15. **Error messages / stack traces**
16. **HTTP headers**
17. **SMB share/file contents**
18. **FTP files/directories**
19. **SNMP information**
20. **LDAP/AD enumeration**
21. **Email addresses discovered from any service**
22. **Database contents / usernames from SQLi**

## Passwords
- mostly lowercase
Here is a possible list
1. username
2. second name
3. softwarename
4. words that can see on login or registering page
5. tagline
6. awkward word in the site
7. box name
8. downloaded file metadata value

- website words
```zsh
curl http://$ip -s | tr '[:upper:]' '[:lower:]' | grep -oE '\b\w{6,}\b' | sort -u > words.txt  
head -n 200 words.txt > custom.txt
```

Here is the extended list
- Username
- First name
- Last name
- First + last name
- 
- Software name
- Service name
- Hostname / machine name
- Domain name
- Company / organization name
- Words visible on login/register pages
- Tagline
- Unusual/awkward words on the site
- Downloaded-file metadata
- Author / developer names
- Email address / email username
- Blog/article author names
- Comments / forum usernames
- Page titles / headings
- Project/application name
- Version number
- Year shown on the site
- Important dates / numbers shown on the site
- Location / city / country names
- Filename / directory names
- Words found in HTML/source comments
- Words found in JavaScript/config files
- `word + year`
- `word + number`
- `word + !/@/#`
- Capitalization variations
- Leetspeak variations
- Keyboard-pattern passwords
- Username + number/year/symbol
- Company/service + number/year/symbol
- Blank password with known username
- Previously discovered password against other usernames/services

## Possible other guesses
### Exploit
1. Port combination
2. Service behavior
3. HTTP headers
4. HTTP response structure
5. Endpoints/features
6. Application fingerprint
7. Protocol fingerprinting
8. OS fingerprinting
9. Nmap NSE enumeration
10. Error messages
11. Configuration leakage
12. File/directory names
13. Protocol features
14. CVE exploit prerequisites
15. Exploit source/code analysis
### Endpoints
1. `robots.txt`
2. `sitemap.xml`
3. `sitemap_index.xml`
4. `security.txt`
5. `/.well-known/`
6. HTML source
7. JavaScript files
8. JavaScript source maps (`.map`)
9. HTML/JS comments
10. Hidden HTML forms
11. Hidden form parameters
12. HTTP headers
13. Cookies
14. Error messages / stack traces
15. Search-engine indexed URLs
16. Web archives / historical URLs
17. API documentation — Swagger/OpenAPI
18. API schemas
19. GraphQL endpoints / introspection
20. API responses containing URLs
21. Redirect `Location` headers
22. Links in downloaded files
23. Git repository information
24. Source-code repository references
25. Framework-specific routes/files
26. Browser developer-tools network requests
27. `OPTIONS` / allowed HTTP methods
28. CORS responses
29. WebSocket endpoints
30. Service-worker files
31. `manifest.json`
32. RSS/Atom feeds
33. XML feeds
34. Documentation/help pages
35. Changelog/release pages
36. Error/debug endpoints
