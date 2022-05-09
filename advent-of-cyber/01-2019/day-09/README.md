# Advent of Cyber 01 - 2019
# Day 09
# Requests

> Machine IP : `10.10.169.100`


the code for getting flag :
```
host = 'http://10.10.169.100:3000/'
path = ''
value = ''


while (path !=  'end'):
        response = requests.get(host+path)
        parsed = response.json()
        value = value + parsed['value']
        path = parsed['next']


print("Your flag : {}".format(value))


```

| Que$t1on$ | An$wer$ |
|-----------|---------|
| What is the value of the flag ? | sCrIPtKiDd |
