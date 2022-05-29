# Madness
# 29-05-2022
# little human
# level : easy


> Machine IP : `10.10.135.203`

> nmap scanning 
```
nmap -A -T5 10.10.135.203 -oN nmap.txt 
```
> in the site we can see a thm.jpg file
```
wget http://10.10.135.203/thm.jpg
```
> check the file exiftool
```
exiftool thm.jpg
```
> we can see the file signature is not ok. lets change it
```
hexeditor thm.jpg
```
> now change the file header to 
````
FF D8 FF E0 00 10 4A 46
49 46 00 01
```

> now open the file and we get the hidden directory into that image.

> create a python file to get the hidden secret page
```
import requests

for i in range(1,100):
    a = requests.get("http://10.10.135.203/th1s_1s_h1dd3n/?secret="+str(i)
    if "wrong" not in a.text:
        print(a.text)s

```
> now using that password extract the file from `thm.jpg`
```
steghide extract -xf thm.jpg
```
> we got the username which is encrypted with ROT10 algo. just decrypt it.

> now downlaod the joker image from the home page of this lab. and also extract the hidden file from that image using steghide. there is no password
```
steghide extract -sf 5iW7kC8.jpg

```
> now we have got the username and the password for ssh.

> after login just search for SUID bit set binary
```
find / -type f -user root -perm -04000 2>/dev/null
```
> here we can see that one binary `screen-4.5.00` has the suid bit set
> now in our local machine we can search for exploit for that binary
```
searchploit screen 4.5.0
```
> we can see that there is one script with local privilege escalation. we will use that script to escalate root privilege. just transfer the file to the vm using `scp` or `http.server` module of python and run the binary to get a root shell.

> now in the root shell we've just got search for the root.txt file
```
find / -type f -name root.txt 2>/dev/null
```


## Flag Submission

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| user.txt | THM{d5781e53b130efe2f94f9b0354a5e4ea} |
| root.txt | THM{5ecd98aa66a6abb670184d7547c8124a} |





#        THE END 
