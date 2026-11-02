---
title: DriftingBlues6
date: 2026-09-05
tags:
  - lab
  - "#version"
Walkthrough: https://portal.offsec.com/machine/driftingblues6-40277/overview/details
---
# DriftingBlues6
## Attack Path
- web enumeration --> found robots.txt and credential zip file
- robots.txt --> reveal `/textpattern/textpattern` endpoint 
- zip file password brute force --> credential for textpattern cms admin
- CMS version --> reveal RCE via file upload --> reverse shell
- kernel version --> vulnerable for Dirty COW' 'PTRACE_POKEDATA' Race Condition Privilege 
## Notes
I use php reverse shell and manually exploit instead of relying on Exploit-DB PoC. Since web root is different

## What Need to Develop
- Patient. Follow 20 min rules.
- If not found a way, get a hint since clock is ticking
- John usage for zip files
- linux recon cheat sheet
- Select correct exploit
## What Learn New
- John for cracking zip files password [[John]]
- Selecting correct exploit
