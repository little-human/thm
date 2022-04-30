# Content Discover

## What is Content Discover

| Questions | Answers |
|-----------|---------|
| What is the Content Discovery method that begins with M? | Manually |
| What is the Content Discovery method that begins with A? | Automated |
| What is the Content Discovery method that begins with O? | OSINT |

## Manual Discovery - Robots.txt


| Questions | Answers |
|-----------|---------|
| What is the directory in the robots.txt that isn't allowed to be viewed by web crawlers ? | /staff-portal

## Manual Discovery - Favicon

| Questions | Answers |
|-----------|---------|
| What framework did the favicon belong to ? | cgiirc |

## Manual Discovery - Sitemap.xml

| Questions | Answers |
|-----------|---------|
| What is the path of the secret area that can be found in the sitemap.xml file ? | /s3cr3t-area |

## Manual Discovery - HTTP Headers

| Questions | Answers |
|-----------|---------|
| What is the flag value from the X-FLAG header ? | THM{HEADER_FLAG} |

## Manual Discovery - Framework Stack

| Questions | Answers |
|-----------|---------|
| What is the flag from the framework's administration portal ? | THM{CHANGE_DEFAULT_CREDENTIALS} |


## OSINT - Google Hacking/Dorking


| Questions | Answers |
|-----------|---------| 
| What google dork operator can be used to only show results from a particular site ? | site: |


## OSINT - Wappalyzer


| Questions | Answers |
|-----------|---------| 
| What online tool can be used to identify what technology a website is running ? | Wappalyzer |

## OSINT - Wayback Machine


| Questions | Answers |
|-----------|---------| 
| What is the website address for the Wayback Machine ? | https://archive.org/web/ |

## OSINT - Github


| Questions | Answers |
|-----------|---------| 
| What is git ? | version controll system |

## OSINT - S3 Buckets

| Questions | Answers |
|-----------|---------| 
| What URL format do amazon s3 buckets end in ? | .s3.amazonaws.com |

## Automated Discovery

used the command 
```
gobuster dir -u  http://10.10.171.246 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x log -t 40

```

| Questions | Answers |
|-----------|---------| 
| What is the name of the directory beginning "/mo..." that was discovered ? | /monthly |
| What is the name of the log file that was discovered ? | /development.log |
