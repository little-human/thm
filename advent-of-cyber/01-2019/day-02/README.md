# Advent of Cyber 01 - 2019
# Day 02
# Arctic Forum 

> Machine IP : `10.10.228.39`

to find the hidden page we will use gobuster :
```
gobuster dir -u http://10.10.228.39:3000 -w /usr/share/wordlists/dirb/common.txt -t 90 | tee dir.txt
```

> just see the page source of /sysAdmin for the hint.


| Que$t1on$ | An$wer$ |
|-----------|---------|
| What is the path of the hidden page? | /sysAdmin |
| What is the password you found ? | defaultpass |
| What do you have to take to the 'partay' | eggnog |
