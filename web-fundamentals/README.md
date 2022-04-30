# Web Fundamental

# How do we load websites ?
|Question|Answers|
|------|--------|
| What request verb is used to retrieve page content? | GET |
| What port do web servers normally listen on? | 80 |
| What's responsible for making websites look fancy? | css |

# MOre HTTP-Verbs and reQuest formats

``` https://developer.mozilla.org/en-US/docs/Web/HTTP/Status 
```

| Question | Answers |
|-----|----------|
| What verb would be used for a login ? | POST |
| What verb would be used to see your bank balance once you're logged in? | GET |
| Does the body of a GET request matter ? Yea/Nay | Nay |
| What's the status code for 'I'm a teapot" ? | 418 |
| What status code will you get if you need to authenticate to access some content, and you're unauthenticated ? | 401 |


# Cookies, tasty! 

``` https://developer.mozilla.org/en-US/docs/Web/HTTP/Status 
```

# Mini CTF 

| Questions | Answers | Commands |
|------|------|--------|
| What's the GET flag ? | thm{162520bec925bd7979e9ae65a725f99f} | curl http://10.10.55.196:8081/ctf/get |
| What's the Post flag ? | thm{3517c902e22def9c6e09b99a9040ba09} | curl -X http://10.10.55.196:8081/ctf/post -d "flag_please" |
| What's the "Get a cookie" flag ? | thm{91b1ac2606f36b935f465558213d7ebd} | curl http://10.10.55.196:8081/ctf/getcookie -c cookie_data |
| What's the "Send a cookie" flag ? | thm{c10b5cb7546f359d19c747db2d0f47b3} | curl -b 'flagpls=flagpls' http://10.10.55.196:8081/ctf/sendflag |


# THE END #
