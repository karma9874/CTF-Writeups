# Challenge Title: Web-1

## Description
May the cURL be with you xD

Flag format: darkCTF{FLAG}

[Link](http://web1.darkarmy.xyz/)

## Solution

On opening the link the site says *to login move to /login by post and send json request* and as the description say to use cURL so lets start with it 

```
curl -X POST http://web1.darkarmy.xyz/ -H 'content-type: application/json' --data '{"user":"karma","pass":"karma"}'
```
In return, we get a jwt token

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/login1.JPG)

Used [jwt.io](https://jwt.io/) to decode that token which had payload like this `{"user":"guest"}`.So what now?

Always check robots.txt of all web challs in any CTF because sometime it may have some data which can be useful.

Just like in this chall accessing `/robots.txt` we get `/flag` page

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/robots1.JPG)

We cant make a GET request to `/flag` so maybe POST?

On making a POST request to `/flag` we get something like this `gimme token and I will give you the flag`

On passing the jwt token which we got earlier we get a response like this `Not the admin, no flag for you`. So it needs admin token, well we can edit the token to `{"user":"admin"}` using [jwt.io](https://jwt.io/) and passing that token to `/flag` we get the flag

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/Dark-PreCTF/Images/flag1.JPG)

flag -> `darkCTF{345y_p345y_JWTs}`

## What was the Bug?
In this chall I was just checking the decoded value to have value of `user` to be `admin` (`{"user":"admin"}`) to show to the flag thats it, the code was not checking for any signature verification