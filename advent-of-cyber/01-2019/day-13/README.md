# Advent of Cyber 01 - 2019
# Day 13
# Accumulate

> Machine IP : `10.10.172.233`

gobuster scan :
```
gobuster dir -u http://10.10.172.233/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 90 | tee dir.txt
```


by looking at the source code of the page we found that one username is `wade` and pasword is `parzival`.


| Que$t1on$ | An$wer$ |
|-----------|---------|
| A web server is running on the target. What is the hidden directory which the website lives on? | /retro |
| Gain initial access and read the contents of user.txt ? |
| Elevate privileges and read the content of root.txt | THM{COIN_OPERATED_EXPLOITATION} |
