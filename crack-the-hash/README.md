# Crack The Hash


## Level 1

cracking the $2y$ hash 
``` 
grep -o -w "\w\{4\}" /usr/share/wordlists/rockyou.txt | grep -w '[[:lower:]]*' > four_digit.txt

cat four_digit.txt| sort | uniq > avail.txt

hashcat -m 3200 hash.txt avail.txt
```
| Hashes | Text | Types |
|-----------|---------|-----|
| 48bb6e862e54f2a795ffc4e541caed4d | easy | md5 |
| CBFDAC6008F9CAB4083784CBD1874F76618D2A97 | password123 | sha1 |
| 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032 | letmein | sha256 |
| $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom | bleh | bcrypt (mode-3200) |
| 279412f945939ba78ce0758d3fd83daa | Eternity22 | md4 |


## Level 2

cracking the third hash
```
cat /usr/share/wordlists/rockyou.txt| sort | uniq | grep -o -w "\w\{6\}" > rock-you.txt

hashcat -m 1800 lvl_2.txt rock-you.txt
```

cracking the last hash 
```
cat /usr/share/wordlists/rockyou.txt| sort | uniq | grep -o -w "\w\{12\}" > rock-you-last-hash.txt

hashcat -m 160 last.txt rock-you-last-hash.txt
```

| Hashes | Text | Types |
|-----------|---------|-----|
| F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85 | paule | sha256 |
| 1DFECA0C002AE40B8619ECF94819CC1B | n63umy8lkf4i | NTLM | 
| $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02. | waka99 | sha512crypt (mode-1800) |
| e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme | 481616481616 | HMAC-SHA1 |
