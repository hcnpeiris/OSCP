---
title: ColdBoxEasy
date: 2026-09-01
tags:
  - lab
  - "#gobuster"
  - "#wordpress"
  - "#SUID"
  - linpeas
  - wpscan
---
# ColdBoxEasy
## Attack Path
* Dir discovery → found `/hidden` endpoint → revealed admin username (`c0dd0`) + other usernames
* Brute-forced admin password → gained access to admin page
* Modified PHP 404 template → reverse shell → `local.txt`
* Ran LinPEAS → found `find` with SUID bit set
* Privilege escalation via SUID `find`
## Notes
`hydra` may show false positive results for brute force, use Burp intruder or WPScan instead.
Reason you can modify php files, WordPress configured for ` define('DISALLOW_FILE_EDIT', true);`

## What Need to Develop
- Not reading the results correctly. - Missed  /hidden endpoint but showed up in Gobuster results.
- WordPress related common attacks
## What Learn New
- [[WPScan]] usage and WordPress related common attacks
- Don't recursively scan if not needed- Time consuming dirbuster recursively scan
- Don't debug tools , move to another one - Spend more time with `hydra` false positive results
- 20 mins break and review works
