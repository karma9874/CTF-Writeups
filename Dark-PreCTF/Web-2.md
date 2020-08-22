# Challenge Title: Web-2

## Description
Okay this time I added the login form and I m verifying the tokens too

Flag format: darkCTF{FLAG}

[Link](http://web2.darkarmy.xyz/)

## Solution

On opening the link we see a login form, log in with any junk user and pass we get redirected to `/flag` which says `Not admin, no flag for you` hmmm...

On checking cookies we see there is key `token` with some value which is JWT again xD, checking it on [jwt.io] get this

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/token2.JPG)

The token is using RS256 algorithm which means it needs a private key and a public key 

On accessing `/robots.txt` and a little bit of scrolling xD we see there is some PRIVATE KEY   

So we have the private key now then we can build a token and apply the signature to it. I used nodejs jsonwebtoken to do this stuff 

```
const jwt = require('jsonwebtoken')
var fs = require('fs')
var private_key = fs.readFileSync('./private_key.key');
var token = jwt.sign({ 'user': 'admin' },private_key, { algorithm:'RS256',noTimestamp:true}); 
console.log(token)
```
![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/node2.JPG)

Now access the /flag using the token as cookie and we get the flag

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/flag2.JPG)

flag -> `darkCTF{n07_4_900d_p14c3_f0r_prv473k3y}`

## What was the Bug?
Hide your private key xD