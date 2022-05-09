# Advent of Cyber 01 - 2019
# Day 06
# Data Elf-iltration 

> follow the DNS UDP stream and convert the hexadecimal to ascii to see the text

> Downlaod the objects in HTTP and crack the password for the application.zip
```
zip2john christmaslists.zip > hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

> extract data from the TryHackMe.jpg using steghide
```
steghide extract -sf TryHackMe.jpg
```



| Que$t1on$ | An$wer$ |
|-----------|---------|
| What data was exfiltrated via DNS ? | Candy Cane Serial Number 8491 |
| What did Little Timmy want to be for christmas ? |
| What was hidden within the file ? | RFC527 |
