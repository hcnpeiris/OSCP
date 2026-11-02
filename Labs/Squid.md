---
title: Squid
date: 2026-09-04
tags:
  - lab
  - "#fullpowers"
  - "#printspoofer"
  - bash
  - winenum
Walkthrough:
---
# Squid
## Attack Path
- Port Scan --> Squid proxy
- [Spose](https://github.com/aancw/spose) scanner--> found some internal ports - MySQL and WAMP server
- WAMP default credentials --> web shell using admin panel
- Using [Fullpowers](https://github.com/itm4n/FullPowers) - Recover default privileges for `LOCAL SERVICE`
- Using [PrintSpoofer](https://github.com/itm4n/printspoofer) and with `SeImpersonatePrivilege` to `SYSTEM`

## Notes
This box can't be rooted with only OSCP theories. You may have to research. 
Especially for how to escalate privileges from local service/network account to system and how to do an internal port scan on squid proxy

## What Need to Develop
- Privilege escalation - LOCAL NETWORK/SERVICE to SYSTEM
- Research during pentest.
- Cross compile related things

## What Learn New
- [[Bash]] loops
- [[Tools/Windows Privilege Escalation]]
- Recovering default privileges of local service/network account. [Fullpowers](https://github.com/itm4n/FullPowers)
- From LOCAL NETWORK/SERVICE to SYSTEM by abusing `SeImpersonatePrivilege` on Windows 10 and Server 2016/2019. [PrintSpoofer](https://github.com/itm4n/printspoofer)
- Interactive shell with `nc`
