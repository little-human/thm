# Metasploit Intro
# 11-05-2022
# little human
# level : Easy


## Main Component of Metasploit

Singles: Self-contained payloads (add user, launch notepad.exe, etc.) that do not need to download an additional component to run.

Stagers: Responsible for setting up a connection channel between Metasploit and the target system. Useful when working with staged payloads. “Staged payloads” will first upload a stager on the target system then download the rest of the payload (stage). This provides some advantages as the initial size of the payload will be relatively small compared to the full payload sent at once.


Stages: Downloaded by the stager. This will allow you to use larger sized payloads.



| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the name of the code taking advantage of a flaw on the targt system ? | Exploit |
| What is the name of the code that runs on the target system to achieve the attacker's goal ? | Payload |
| What are self contained payloads called ? | singles |
| Is "windows/x64/pignback_reverse_tcs" among singles or staged payload ? | singles |


## Msfconsole


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What would you search for a module related to Apache | search Apache |
| Who provided the auxiliary/scanner/ssh/ssh_login module ? | todb |

## Working with modules


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How would you set the LPORT value to 6666 ? | set LPORT 6666 |
| How would you set the global value for RHOSTS to 10.10.19.23 ? | setg RHOSTS 10.10.19.23 |
| What command would you use to clear a set payload ? | unset payload | 
| What command do you use to proceed with the exploitation phase ? | exploit |



#        THE END 
