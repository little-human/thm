# Advent of Cyber 01 - 2019
# Day 01

> Machine IP : `10.10.153.43`


open the link in browser and look at the cookies
```
http://10.10.153.43:3000
```
> First we will register as another user to see the cookies format and then login with that user.

> cookie value : YWRtaW52NGVyOWxsMSFzcw%3D%3D

> we need to decode `mcinventoryv4er9ll1!ss` in base64 format and replace the `==` with `%3D%3D` and then just set as cookie and refresh the page to see the list.


| Que$t1on$ | An$wer$ |
|-----------|---------|
| What is the name of the cookie used for authentication ? | authid |
| If you decode the cookie, what is the value of the fixed part of the cookie? | v4er9ll1!ss |
| After accessing his account, what did the user mcinventory request? | firewall |
