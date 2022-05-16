# HTTP in detail
# 16-05-2022
# little human
# level Easy 



## What is HTTP(S)? 

> Just cllick on the link on the webpage to obtain the flag

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What does the HTTP stands for ? | Hyper Text Transfer Protocol |
| What does the S in HTTP Stands for ? | Secure |
| On the mock webpage on the right there is an issue, once you've fount it, click on it. What is the challenge flag ? | THM{INVALID_HTTP_CERT} |

## Request and Responses 


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What HTTP protocol is being used in the above example ? | HTTP/1.1 |
| What response header tells the browser how much data to expect ? | content-length |

## HTTP Methods

| Methods | Use |
|---------|-----|
| GET | For getting informatin from a web server |
| POST | To submit or secure data to the server |
| PUT | To update previous information into the srever |
| DELETE | To delete previous data form the server |

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What method would be used to create a new user account ? | POST |
| What method would be used to update your email address ? | PUT |
| What method would be used to remove a picture you've uploaded to your account ? | DELETE |
| What mehtod would be used to view a news article ? | GET |

## HTTP Status Codes 

| CODE | DEF |
|------|-----|
| 1XX | Information |
| | |
| 2XX | Success |
| 200 | OK |
| 201 | Created  |
| | |
| 3XX | Redirect |
| 301 | Permanent Redirect |
| 302 | Temporary Redirect |
| | |
| 4XX | Client Error |
| 400 | Bad Request |
| 401 | Not Authorised |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| | |
| 5XX | Server Error |
| 500 | Internal Server Error |
| 503 | Service Unavailable |


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What response code might you receive if you've created a new user or blog post article ? | 201 |
| What response code might you receive if you've tried to access a page that doesn't exists ? | 404 |
| What response code might you receive if the web server cannot access its database and the application crashes ? | 503 |
| What response code might you receive if you try to edit your profile without logging in first ? | 401 |



## Headers


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What headers tells the web server what browser is being used ? | user-agent |
| What header tells the browser what type of data is being returned | content-type |
| What header tells the web server which website is being requested ? | HOST |

## Cookies 


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Which header is used to save cookies to your computer ? | set-cookie |

## Making Requests 


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| Make a GET request to /room | THM{YOU'RE_IN_THE_ROOM} |
| Make a GET request to /blog and using the gear icon set the id parameter to 1 in the URL field | THM{YOU_FOUND_THE_BLOG} |
| Make a DELETE Request to /usr/1 | THM{USER_IS_DELETED} |
| Make a PUT request to /user/2 with the username parameter set to admin | THM{USER_HAS_UPDATED} |
| POST the username of thm and a password of letmein to /login | THM{HTTP_REQUEST_MASTER} |





#        THE END 
