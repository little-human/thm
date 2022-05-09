# Advent of Cyber 01 - 2019
# Day 03
# Evil Elf

> filter data using `tcp.stream eq 1` and follow the telnet stream

> we got the hash for the user buddy : `$6$3GvJsNPG$ZrSFprHS13divBhlaKg1rYrYLJ7m1xsYRKxlLh0A1sUc/6SUd7UvekBOtSnSyBwk3vCDqBhrgxQpkdsNN6aYP1`

> we will use hashcat to crack this password :
```
hashcat -m 1800 hash.txt /usr/share/wordlist/rockyou.txt
```

| Que$t1on$ | An$wer$ |
|-----------|---------|
| Whats the destination IP on packet number 998 ? | 63.32.89.195 |
| What item is on the Christmas list? | ps4 |
| Crack buddy's password! | rainbow |
