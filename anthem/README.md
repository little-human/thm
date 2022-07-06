# Anthem
## 06-06-2022
## level : easy

Machine IP : `10.10.25.33`

## Website Analysis

Scanning the target with Nmap
```
nmap -sV 10.10.25.33 -Pn -oN anthem.txt
```

Running the gobuster 
```
gobuster dir -u http://10.10.25.33/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 100 -x txt,php| tee data.txt
```
Extracting all paths with a status code 200
```
cat data.txt | grep -e "(Status: 200)" | grep -o -e "/[A-Za-z0-9]*\.*[a-z]*" > paths 
```

Finding all the mails : 
```
for line in $(cat paths); do curl -s http://10.10.25.33$line | grep -oie "[A-Za-z0-9]*\@.*\.com" ; done

```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What port is open for the web server ? | 80 |
| What port is for remote desktop service ? | 3389 |
| What is the possible password in one of the pages web crawlers check for ? | UmbracoIsTheBest! |
| What CMS is the website using ? | Umbraco |
| What is the domain of the website ? | anthem.com |
| What's the name of the Administrator ? | Solomon Grundy |
| Can we find the email address of the administrator ? | sg@anthem.com |


## Spot the flag

As we've seen that the username in email is `SG` and we found a password previously `UmbracoIsTheBest!`

Lets use remmina for Login to that Machine


Finding Flag using curl
```
for line in $(cat paths); do curl -s http://10.10.25.33$line | grep -o -e "THM{.*}" ; done
```


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is flag 1? | THM{L0L_WH0_US3S_M3T4} |
| What is flag 2? | THM{G!T_G00D} |
| What is flag 3? | THM{L0L_WH0_D15} |
| What is flag 4? | THM{AN0TH3R_M3TA} |


## Final steps

The hidden forlder at `C:\` containt the file into `backup` folder.

Add the permession for user

and You can read it.


After that Login again as `Administrator`

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Gain initial access to the machine, what is the contents of user.txt ? | THM{N00T_NO0T} |
| Can we spot the admin password? | ChangeMeBaby1MoreTime |
| Escalate your privileges to root, what is the contents of root.txt ? | THM{Y0U_4R3_1337} |




Thank You for Reading this





#    THE END    
