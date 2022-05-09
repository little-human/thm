# Advent of Cyber 01 - 2019
# Day 04
# Training 

> find "string" in files : `grep password *`
> find ip in files : `grep -E '[0-9]{2}\.[0-9]\.[0-9]\.[0-9]{2}' *`
> find how many user have access : `cat /etc/passwd | grep home`
> Calculate sha1 hash of file8 : `sha1sum file8`

> look for shadow backup file to see the hash of the password : `locate shadow.bak`




| Que$t1on$ | An$wer$ |
|-----------|---------|
| How many visible files are there in the home directory(excluding ./ and ../)? | 8 |
| What is the content of file5 ? | recipes |
| Which file contains the string 'password' ? | file6 |
| What is the IP address in a file in the home folder ? | 10.0.0.05 |
| How many users can log into the machine ? | 3 |
| What is the sha1 hash of file8 ? | fa67ee594358d83becdd2cb6c466b25320fd2835 |
| What is mcsysadmin's password hash? | $6$jbosYsU/$qOYToX/hnKGjT0EscuUIiIqF8GHgokHdy/Rg/DaB.RgkrbeBXPdzpHdMLI6cQJLdFlS4gkBMzilDBYcQvu2ro/ |
