# Challenge Title: Web-4

## Description
This time I have made sure that no secrets will be public but I still think its too weak.

Flag format: darkCTF{FLAG}

[Link](http://web4.darkarmy.xyz/)

## Solution

Seeing the description we know it should be related to jwt secret brute-forcing, anyway, let's start with it

Log in with junk value, check cookies, use [jwt.io](http://jwt.io) on token value 

On checking cookies we see there is key `token` with some value, checking it on [jwt.io]() get this

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/token4.JPG)

The token is using HS256algorithm with means it is using a secret_key for encryption, on accessing `/robots.txt` we see nothing now xD

So seeing the description we know it should be related to brute-forcing for secret_key as there is nothing else xD

To do that I used  this tool [jwt_tool](https://github.com/ticarpi/jwt_tool) with `rockyou.txt`

```
python3 jwt_tool.py eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiZ3Vlc3QifQ.1mGqtRViJqKA-TYFeNCcSkgLinKt9CgBnGxjL9KXtNA

```

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/bf.JPG)

Now we have the secret_key which is `redraider`

So now we build a token of HS256 with secret_key as `redraider` with value of user as admin.

```
const jwt = require('jsonwebtoken')
var token = jwt.sign({ 'user': 'admin' },'redraider', { algorithm:'HS256',noTimestamp:true}); 
```
![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/node4.JPG)

Now access the /flag using the token as cookie and we get the flag

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/flag4.JPG)

flag -> `darkCTF{5ymm37r1c_k3y_cr4ck1n9}`

## What was the Bug?
Don't use secret which can be cracked by bruteforcing it
