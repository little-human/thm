# Ice
# 11-05-2022
# little human


# Recon 

> Machine IP : `10.10.96.25`

> Nmap Scanning : 
```
nmap -A -T5 10.10.96.25 -oN nmap.txt
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| One of the more interesting ports that is open is Microsoft Remote Desktop (MSRDP). What port is this open on ? | 3389 |
| What service did nmap identify as running on port 8000 ? (First word of this service) | Icecast |
| What does Nmap identify as the hostname of the machine ? (All caps for the answer) | DARK-PC |


# Gain Access

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What type of vulnerability is it? | Execute code overflow |
| What is the CVE Number for this vulnerability? This will be in the format:CVE-0000-0000 | CVE-2004-1561 |
| What is the full path for the exploitation module in Metasploit ? | exploit/windows/http/icecast_header |
| What is the only required setting which currently is blank ? | RHOSTS |


# Escalate

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What's the name of the shell we have now ? | meterpreter |
| What user was running that Icecast process ? | DARK |
| What build of windows is the system ? | 7601 |
| Now that we know some of the finer details of the system we are working with, let's start escalating our privileges. First, what is the architecture of the process we're running ? | x64 |
| Running the local exploit suggester will return quite a few results for potential escalation exploits. What is the full path (starting with exploit/) for the first returned exploit? | exploit/windows/local/bypassuac_eventvwr |
| Now that we have set our session numbe, further options will be revealied in the options menu. We'll have to set one more as our listener IP isn't correct. What is the name of this options ? | LHOST |
| We can now verify that we have expanded permisssions using the command 'getprivs'. What permission listed allows us to take ownership of files ? | SeTakeOwnershipPrivilege |


# Looting

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the name of the printer service ?  | spoolsv.exe |
| Let's check what user we are now with the command 'getuid'. What user is listed ? | NT AUTHORITY\SYSTEM |
| Which command allows up to retrieve all credentials? | creds_all |
| What is Dark's password ? | Password01! |


# Post-Esploitation

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What command allows us to dump all of the password hashes stored on the system ? | hashdump |
| While more useful when interacting with a machine being used, what command allows us to watch the remote user's desktop in real time ? | screenshare |
| How about if we wanted to record from a microphone attached to the system ? | record_mic |
| To complicate forensics efforts we can modify timestamps of files on the system. What command allows us to do this ? | timestomp |
| Mimikatz allows us to create what's called a 'golden ticket', allowing us to authenticate anywhere with ease. What command allows us to do this? | golden_ticket_create |






#        THE END 
