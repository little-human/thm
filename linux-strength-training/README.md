# Linux Strength Training
# 19-05-22
# little human
## Level : Easy
> Machine IP : `10.10.206.160`

## Finding your way around linux - overview


| What we can do | Syntax | Example |
|----------------|--------|---------|
| Find files based on filename | find [path] -type f -name [filename] | find /home/deva/ -type f -name flag.txt |
| Find directory based on directory name | find [path] -type d -name [name] | find /home/deva/ -type d -name thm |
| Find files based on size | find /home/dev -type f -size [size] | find /home/deva/ -type f -size 10c (c - bytes, k - kb, M - mb, G - gb) |
| Find files based on username | find [path] -type f -user [username] | find /home/deva/ -type f -user root |
| Find files based on group | find [path] -type f -group [group] | find / -type f -group [group name] | 





> Finding the flag 
```
find / -type f -name additionalHINT 2>/dev/null
find /home/topson -type d -name "telephone numbers"
nano workflows/xft/eBQRhHvx
```
now just search for `{` with the keyboar combination of `CTRL+W`

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the correct options for finding files based on group | -group |
| What is format for finding a file with the user named Francis and with a size of 52 kilobytes in the directory /home/francis/ | find /home/francis -type f -user francis -size 52k |
| SSH as topson using his password topson. Go to the /home/topson/chatlogs directory and type the following : grep -iRl 'keyword'. What is the name of the file you found using the command ? | 2019-10-11 |
| What are the characters subsequent to the word you found ? | ttitor |
| Read the file named 'ReadMeIfStuck.txt'. What is the flag ? | Flag{81726350827fe53g} |

## Working with files 


> getting the second flag 
```
mv corperateFiles/RecordsFinances/-MoveMe.txt corperateFiles/RecordsFinances/-march\ folder/
cd corperateFiles/RecordsFinances/-march\ folder/
./-runME.sh
```




| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Hypothetically, you find yourself in a directory with many files and want to move all these files to the directory of /home/francis/logs. What is the correct command to do this ? | mv * /home/francis/logs |
| Hypothetically, you want to transfer a file from your /home/james/Desktop/ with the name script.py to the remote machine(192.168.10.5) directory of /home/john/scripts using the username john. what would be the full command to do this? | scp /home/james/Desktop/script.py john@192.168.10.5:/home/john/scripts |
| How would you rename a folder named -logs to -newlogs | mv -logs -newlogs |
| How would you copy the file named encryption keys to the directory of /home/john/logs | cp "encryption keys" /home/john/logs |
| Find a file named readME_hint.txt inside topson's directory and read it. Using the instructions it gives you, get the second flag | Flag{234@i4s87u5hbn$3} |

## Hashing Introduction

> finding the hashes 
```
find / -type f -name hashC.txt 2>/dev/null

```
> seeing the hashtype 
```
cat hash.txt | hash-identifier
```
> cracking the hash using john the ripper 
```
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```
> Cracking the hashC.txt 
```
john --wordlist=ww.mnf hashC.txt --format=raw-sha256
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Download the hash file attached to this task and attempt to crack the MD% hash. What is the password? | secret123 |
| What is the hash type stored in the file hashA.txt ? | MD4 |
| Crack hashA.txt using john the ripper, what is the password ? | admin |
| What is the type of the hash stored in the file hashB.txt | SHA-1 |
| Find a wordlist with the file extension of '.mnf' and use it to crack the hash with the filename hashC.txt. What is the password ? | unacvaolipatnuggi |
|  Crack hashB.txt usign johnt the ripper, what is the password ? | letmein |

## Decoding base64

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the name of the tool which allows us to decode base64 strings ? |
| find a file called encoded.txt. What is the special answer ? | john |

## Encryption / Decryption using gpg 

> encrypt using gpg with AES-256 algo
```
gpg --cipher-algo AES-256 --symmetric secret.txt
```
> decrypt usign gpg 
```
gpg secret.txt.gpg
```
| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| You wish to encrypt a file called history_logs.txt using the AES-128 scheme. What is the full command to do this ? | gpg --cipher-algo AES-128 --symmetric history_logs.txt |
| What is the command to decrypt the file you just encrypted ? | gpg history_logs.txt.gpg |
| Find an encrypted file called layer4.txt, its password is bob. Use this to locate the flag. What is the flag ? | Flag{B07$f854f5ghg4s37} |

## Cracking encrypted gpg files 

> cracking the file password (first bring the files to local machine )
```
gpg2john personal.txt.gpg > hash.txt
tac data.txt > list.txt
john --wordlist=list.txt hash.txt
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Find an encrypted file called personal.txt.gpg and find a wordlist called data.txt Use tac to reverse the wordlist before brute-forcing it against the encrypted file. What is the password to the encrypted file ? | valamanezivonia |
| What is written in this now decrypted file ? | GETTING STRONGER IN LINUX |

## Reading SQL

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Find a file called employees.sql and read the sql database. ( Sarah and Sameer can log both into mysql using the password password). Find the flag contained in one of the tables. What is the flag ? | Flag{13490AB8} |

## Final Challenge

> extracting the wordlist 
```
grep "ebq" * | cut -d ":" -f 2

```
| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is Sameer's SSH password ? | thegreatestpasswordever000 |
| What is the password for the sql databaase back-up copy | ebqattle |
| Find the SSH password of the user James. What is the password ? | vuimaxcullings |
| What is the root flag ? | Flag{6$8$hyJSJ3KDJ3881} |





#        THE END 
