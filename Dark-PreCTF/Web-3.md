# Challenge Title: Web-3

## Description
Ahh you solved the 1st and 2nd part, let's see if you can confuse the web to get the flag

Flag format: darkCTF{FLAG}

[Link](http://web3.darkarmy.xyz/)

## Solution

From the description, it suggests about the jwt key confusion attack.

Log in with junk value, check cookies, use [jwt.io](http://jwt.io) on token value 

On checking cookies we see there is key `token` with some value, checking it on [jwt.io]() get this

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/token3.JPG)

The token is using RS256 algorithm, on accessing `/robots.txt` we see there is some public key

So seeing the description we know it should be jwt confusion attack which is changing the `Asymmetric Cipher Algorithm to Symmetric Cipher Algorithm`

So we need to build an HMAC(HS256) token using the public key as a secret to it. I used nodejs jsonwebtoken to do this stuff 

```
const jwt = require('jsonwebtoken')
var fs = require('fs')
var publicKey = fs.readFileSync('./public_key.key');
var token = jwt.sign({ 'user': 'admin' }, publicKey, { algorithm:'HS256',noTimestamp:true});
console.log(token)
```
![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/node3.JPG)

Now access the /flag using the token as cookie and we get the flag

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/flag3.JPG)

flag -> `darkCTF{5m007h_724n51710n_45ym_2_5ymm}`

## What was the Bug?
At encoding jwt token the code was only using RS256 algorithm but at the time of verifying token it was using RS256 and HS256 both algorithm