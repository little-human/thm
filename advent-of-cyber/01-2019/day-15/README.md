# Advent of Cyber 01 - 2019
# Day 15
# LFI - Local File Inclusion

> Machine IP : `10.10.75.101`


> First intercept the requests using burpsuit
> then replace the notes paths with the path of desired files using url encoded path.

use john to crack the password :
```
unshadow passwd.txt shadow.txt > unshadow.txt

john --wordlist=/usr/share/wordlists/rockyou.txt unshadow.txt

```

> now as we have a username and a password we can connect to the machine using SSH to obtain the flag1.txt 


| Que$t1on$ | An$wer$ |
|-----------|---------|
| What is charlie going to book a holiday for ? | Hawaii |
| Read /etc/shadow and crack Charlies password | password1 |
| What is flag1.txt ?| THM{4ea2adf842713ad3ce0c1f05ef12256d} |
