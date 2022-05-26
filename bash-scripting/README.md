# Bash Scripting
# 24-05-2022
# little human
## level : easy



## Introduction

### What is bash ? 
> Bash is a scripting language that runs within the terminal on most Linux distros, as well as MacOS. Shell scripts are sequence of bash commands within a file, combined together to achieve more comples tasks than simple one-liner and are especially useful when it comes to automating sysadmin task such as backups.





## Our first simple bash scripts


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What piece of code can we insert at the start of a line to comment out our code ? | # |
| What will the following script output to the screen, echo "BishBashBosh" | BishBashBosh |




## Variables

> bash script 
```

#!/bin/bash
name="Jammy"
age=21
echo "$name is $age years old"
city="Paris"
country="France"


```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What would this code return ? | Jammy is 21 years old |
| How would you print out the city to the screen ? | echo $city |
| How would you print out the country to the screen ? | ehco $country |



## Parameters


| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| How can we get the number of arguments supplied to a script ? | $# |
| How can we get the filename of our current script(aka our first argument)? | $0 |
| How can we get the 4th argument supplied to the script ? | $4 |
| If a script asks us for input how can we direct our input into a variable called 'test' using "read" | read test |
| What will the output of "echo $1 $3" if the script was ran with "./script.sh hello hola aloha" | hello aloha |



## Arrays 

> the given array is 
```
cars=('honda' 'audi' 'bmw' 'tesla')
```

| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What would be the command to print audi to the screen using indexing ? | echo "${cars[1]}" |
| If we wnated to remove tesla from the array how would we do so? | unset cars[3] |
| How could we insert a new value called toyota to replace tesla ? | cars[3]='toyota' |




## Conditionals 

| Operator | Description | Math Sign |
|----------|-------------|-----------|
| -eq | equals to | == |
| -ne | not equals to | != |
| -gt | greater than | > |
| -lt | less than | < |
| -ge | greater than or equals to | >= |
| -le | less than or equals to | <= |




| Qu3$t10n$ | 4n$w3r$ |
|-----------|---------|
| What is the flag to check if we have read access to a file ? | -r |
| What is the flag to check to see if it's a directory ? | -d |





#        THE END 
