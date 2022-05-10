# DNS in detail
# 10-05-2022
# little human




## What is DNS ? 

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What does the DNS stands for ? | Domain Name System |


## Domain Hierarchy 

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the maximum length of a subdomain? | 63 |
| Which of the following characters cannot be used in a subdomain(3 b_-)? | _ |
| What is the maximum length of a domain name ? | 253 |
| What type of TLD is .co.uk ? | ccTLD |




## Record Types



| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What type of record would be used to advise where to send email ? | MX |
| What type of record handles IPv6 addresses ? | AAAA |


## Making a Request 

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What field specifies how long a DNS record should be cached for? |
| What type of DNS Server is usually provided by your ISP ? |
| What type of srver holds all the records for a domain? | Authoritative |



## Practical
> DNS lookup
```
nslookup website.thm
```

> CNAME lookup 
```
nslookup --type=CNAME shop.website.thm
```
> TXT record lookup 
```
nslookup --type=TXT website.thm
```
> MX record lookup 
```
nslookup --type=MX website.thm
```
> A record lookup
```
nslookup --type=A www.website.thm
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the CNAME of shop.website.thm? |
| What is the value of the TXT record of website.thm ? | THM{7012BBA60997F35A9516C2E16D2944FF} |
| What is the numerical priority value for the MX record ? | 30 |
| What is the IP address for the A record of www.website.thm? | 10.10.10.10 |





#        THE END 
