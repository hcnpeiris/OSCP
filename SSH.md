SSH keys changed, remove the old entry:
```
ssh-keygen -R IP
```
## Authentication System
- Use asymmetric encryption. 2 keys are generated
- Private key/ `id_rsa` - Generate challenge
- Public key/`id_rsa.pub` - Verify challenge
- On server `authorized_keys` needs to contain user `id_rsa.pub` .
- So server can authenticate/verify the user
- key pair can generate via 
```
ssh-keygen -t rsa -b 4096 ./
```
- `-t` and `-b` are used to specify the algorithm and size.
- Then you need to add public key to server manually or use this command
```
ssh-copy-id -i id_rsa.pub user@host
```
- [SSH Keygen Docs](https://www.ssh.com/academy/ssh/keygen)
- id_rsa permission
```
chmod 600 id_rsa
```
