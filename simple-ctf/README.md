# Simple CTF
ip : `10.10.177.38`
nmap scanning
``` nmap -A -sC -T5 -Pn 10.10.177.38 -oN nmap-scan.txt
```
getting root shell from user
```
sudo vim -c ':!/bin/sh'
```
| Questions | Answers |
|-----------|---------|
| How many services are running under port 1000? | 2 | 
| What is running on the higher port ? | ssh | 
| What's the CVE you're using against the application? | CVE-2019-9053 |
| What kind of vulnerability is the application vulnerable ? | sqli |
| What's the password ? | secret |
| Where can you login with the details obtained ? | ssh |
| What's the user flag ? | G00d j0b, keep up! |
| Is there any other user in the home directory? What's its name ? | sunbath |
| What can you leverage to spawn a privileged shell ? | vim |
| What's the root flag ? | W3ll d0n3. You made it! |
