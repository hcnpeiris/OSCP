---
title: Access
date: 2026-09-07
tags:
  - lab
Walkthrough:
---
# Access
## Attack Path
- Analyze how the website works
- When purchasing tickets ; can upload files and those files on `/uploads` --> can be used for RCE
- Php files are not allowed so can use `.php...` or upload `.htaccess` contain `AddType application/x-httpd-php .evil`
- It tells Apache to treat files ending in .evil as PHP content. 
- Then web shell
- System has another service account for MSSQL. It can be Kerberoast attack --> crack the password.
- Since web shell not interactive, cant use credentials with runas.
- Except SMB, no protocol to authenticate as MSSQL account and SMB not much reveal more.
- Use RunAsCs - It can use direct credential on command --> MSSQL's shell
- MSSQL user has SeManageVolumePrivilege
- Abuse it using https://github.com/CsEnox/SeManageVolumeExploit


## Notes

## What Need to Develop

## What Learn New
- Using `.htaccess` - allows the creation of new legitimate PHP extensions that are allowed by the web application. Useful when .php files are blocked [[Apache]]
- Use RunAsCs to avoid password prompt limitation
