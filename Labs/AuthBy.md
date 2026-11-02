---
title: AuthBy
date: 2026-09-06
tags:
  - lab
Walkthrough:
---
# AuthBy
## Attack Path
ftp anonymous port --> based on files, there are admin and offsec users
brute force ftp --> admin:admin
ftp admin has .htpasswd --> brute force --> credential for Apache server
upload php shell using ftp --> trigger it via Apache server
SeImpersonatePrivilege --> JuicyPotato

## Notes

## What Need to Develop

## What Learn New
