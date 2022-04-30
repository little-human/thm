## Nmap Scanning ##
# Introduction #
>> What networking constructs are used to direct traffic to the right application on a server ?
=> ports
>> How many of the ports are available on any network-enabled computer?
=> 65535
>> How many of these are considered "well-known"?
=> 1024 ( 0 - 1023 )

# Nmap Switches #
>> What is the first switch listed in the help menu for a 'Syn Scan' (more on this later!)?
=> -sS
>> Which switch would you use for a "UDP scan"?
=> -sU
>> If you wanted to detect which operating system the target is running on, which switch would you use?
=> -O
>> Nmap provides a switch to detect the version of the services running on the target. What is this switch?
=> -sV
>> The default output provided by nmap often does not provide enough information for a pentester. How would you increase the verbosity?
=> -v
>> Verbosity level one is good, but verbosity level two is better! How would you set the verbosity level to two?
=> -vv
>> What switch would you use to save the nmap results in three major formats?
=> -oA
>> What switch would you use to save the nmap results in a "normal" format?
=> -oN
>> A very useful output format: how would you save results in a "grepable" format?
=> -oG
>> How would you activate aggressive setting?
=> -A
>> How would you set the timing template to level 5?
=> -T5
>> How would you tell nmap to scann only port 80?
=> -p 80
>> How would you tell nmap to scan ports 1000-1500?
=> -p-
>> How would you activate a script from the nmap scripting library ? 
=> --script
>> How would you activate all of the scripts in the "vuln" category?
=> --script=vuln

# Scan Types Overview #


# TCP Connect Scans #

>> Which RFC defines the appropriate behaviour for the TCP protocol?
=> RFC 793
>> if a port is closed, which flag should the server send back to indicate this?
=> RST

# SYN Scans #

>> There are two other names for a SYN scan, what are they?
=> half-open,stealth
>> Can Nmap use a SYN scan without sudo permissions (Y/N)?
=> N

# UDP Scans #

>> If a UDP port doesn't respond to an Nmap scan, what will it be marked as?
=> open|filtered
>> When a UDP port is closed, by convention the target should send back a "port unreachable" message. Which protocol would it use to do so?
=> icmp

# NULL, FIN and Xmas #

>> Which of the three shown scan types uses the URG flag?
=> xmas
>> why are NULL, FIN and Xmas scans generally used ?
=> firewall evasion
>> Which common OS may respond to a NULL, FIN or Xmas scan with a RST for every port ?
=> Microsoft Windows

# ICMP Network Scanning #

>> How would you perform a ping sweep on the 172.16.x.x network (Netmask: 255.255.0.0) using Nmap? (CIDR notation)
=> nmap -sn 172.16.0.0/16

# NSE Scripts Overview #

>> What language are NSE scripts written in ?
=> lua
>> Which category of scripts would be a very bad idea to run in a production environment?
=> intrusive

# Working with the NSE #

>> What optional argument can the ftp-anon.nse script take?
=> maxlist

# Searching for Scripts #

>> What is the filename of the script which determines the underlying OS of the SMB server?
=> smb-os-discovery.nse
>> Read through this script. what does it depend on ? 
=> smb-brute

# Firewall Evasion #

>> Which simple (and frequently relied upon) protocol is often blocked, requiring the use of the -Pn switch?
=> icmp
>> Which Nmap switch allows you to append an arbitrary length of random data to the end of packets?
=> --data-length

# Practical #

>> Does the target (10.10.219.219)respond to ICMP (ping) requests (Y/N)?
=> N
>> Perform an Xmas scan on the first 999 ports of the target -- how many ports are shown to be open or filtered?
=> 999
>> There is a reason given for this -- what is it?
=> no response
>> Perform a TCP SYN scan on the first 5000 ports of the target -- how many ports are shown to be open?
=> 5
>> Deploy the ftp-anon script against the box. Can Nmap login successfully to the FTP server on port 21? (Y/N)
=> Y




