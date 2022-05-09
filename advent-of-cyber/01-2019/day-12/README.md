# Advent of Cyber 01 - 2019
# Day 12
# Elfcryption

find the md5 hashsum of the note1 :
```
md5sum note1.txt.gpg
```

decrypt note1 with password `25daysofchristmas` :
```
gpg --decrypt note1.txt.gpg
```

decrypt the note2 with openssl :
```
openssl rsautl -decrypt -inkey private.key -in note2_encrypted.txt -out plaintext.txt
```

| Que$t1on$ | An$wer$ |
|-----------|---------|
| What is the md5 hashsum of the encrypted note1 file ? | 24cf615e2a4f42718f2ff36b35614f8f |
| Where was elf Bob told to meet Alice ? | Santa's Grotto |
| Decrypt note2 and obtain the flag! | THM{ed9ccb6802c5d0f905ea747a310bba23} |
