---
title: Amaterasu
date: 2026-09-01
tags:
  - lab
  - linpeas
  - gobuster
  - "#cron"
  - "#bash"
  - "#ssh"
  - ftp
  - "#curl"
---
# Amaterasu
## Attack Path
- Dir discover --> info, help and other endpoint that can list and upload files
- Path traversal on list, upload files can modify, list `/home/alfredo/` and `/tmp`
- Add `id_rsa.pub` to `authorized_keys` --> ssh as Alfredo
- Linpeas --> Root user cron job (every minutes)
- Cron job script can be read. Mentioned `export PATH="/home/alfredo/restapi:$PATH"` and later use `tar`
- Add fake `tar` script on `/home/alfredo/restapi`
## Notes

## What Need to Develop
Failing to recognize that you can change `authorized_keys`
Lack of ssh authentication knowledge
Spending too much time on FTP. It's not bad like previous labs but if I run other enum in parallel, that can save my time.
Bash variables.
## What Learn New
[[SSH|SSH Authentication]]
[[FTP|Basic things about FTP]]
Move to one thing if the other doesn't work rather than digging a dead end.
[[Curl]] - Upload file with multiple field.
