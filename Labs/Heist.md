---
title: Heist
date: 2026-09-08
tags:
  - lab
Walkthrough:
---
# Heist
- Web app --> SSRF
- Use Responder for capture ntlm hash --> cracking --> enox credentials --> evil-winrm
- BloodHound show enox can retrieve apache user password(ReadGMSAPassword) --> research--> bloodyAD --> ntlm hash --> evil-winrm
- apache user has SeRestorePrivilege --> privilege escalation via changing Utilman.exe to cmd.exe
- via rdp AD login page --> use Win + U --> CMD as system --> change Administrator password

## Notes

## What Need to Develop

## What Learn New
SeRestorePrivilege abuse via utilities [Read more](https://www.hackingarticles.in/windows-privilege-escalation-serestoreprivilege/)
