---
title: Robust
date: 2026-09-02
tags:
  - lab
  - "#winenum"
  - sqlite
  - "#mysql"
Walkthrough: https://portal.offsec.com/machine/robust-38783/overview/details
---
# Robust
## Attack Path
- Bypass IP restriction using `X-forwarded-for`
- Discover `/home.php` endpoint by dir brute forcing
- Union based sqli for reveal Jeff's password (`Mathsisfun123`)
- Find credentials via `plum.sqlite`
## Notes
If I only used Firefox. I might miss the 302 response body.

## What Need to Develop
- SQL injection basic
- SQL injection Union payloads
- Windows Plain text enumerations (DBs) 
- Mood control
- Need more familiar with windows enumeration.
- Write the report as soon as the lab is completed.
## What Learn New
- Use `-Force` to find hidden files [[Windows Enumuration]]
- Learn basic syntax of [[SQLite]] and [[SQL Queries]]
