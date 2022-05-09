# Advent of Cyber 01 - 2019
# Day 16
# File Confusion

> to extract all files easily create a python script :
```
import os
import sys


file = sys.argv[1]
os.system('unzip {}'.format(file))

list = os.listdir('.')
for file in list:
  if '.zip' in file  and 'final-final' not in file:
    os.system('unzip {}'.format(file))

```
> move all the text files into a folder :
```
mv *.txt text
```
> find how many files are there :
```
ls | wc -l

```
> find how many files contain Version:1.1 in their metadata :
```
grep "Version>1.1" *.txt
```
> find which file contains the password :
```
grep "password" *.txt
```


| Que$t1on$ | An$wer$ |
|-----------|---------|
| How many files did you extract(excluding all the .zip files) | 50 |
| How many files contain version:1.1 in their metadata? | 3 |
| Which file contains the password ? | DL6w.txt |
