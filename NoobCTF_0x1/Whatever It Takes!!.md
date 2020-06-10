# Challenge Title: Whatever It Takes!!

## Description
Some basic common ciphers can be your soulmates sometime

https://www.youtube.com/watch?v=9nGrDl5_zrc

Given file [chall4.txt](https://raw.githubusercontent.com/AdityaSec/NoobCTF-0x1/master/Crypto/WhatEverItTakes/chall4.txt)

## Solution

In the file there is some hex text and some rubbish 

After decoding the hex we get 
```
gAAAAABe0lyu1kzJxIgHiMeg8njOZm_8H2oJZtMOR-bhp-ozznCFkkY5H3l9BG1dKh7kpf_OKaHRJkWTpzbZuOuoq56-RfEh6AEFuMOYn6egUYdQja7kxymgY286bIHrEEAkbeGJxugOsDfFM-rc7zba5aiNaEZExiWrPW0JW8HSfITTMTrTj86WeqfhSHXk2HxqSL4O53cqXRev4O8uzDjxfpP3KDBx_7JtXasSyJF6VEOJTRwIzdEuVzepALD_SvLrc_eCP0wQKGqPcAo8oqxs4tnLRH5tnqLsdGot7NFXm75EnxBzl8Bb7wYaAnTzSYVdGQSYyOCdDS7rMuJ4o3crIIZ1H0Jba1lou_RFnct1dcG7e_JnjT5nhZZda0ArBzn90Ear2mzan»/úF«®
```
The starting part is `AAAAA` so I quicky thought of [Fernet (Decode)](https://asecuritysite.com/encryption/ferdecode) used this tool to decode that cipher but we need a key to decode it 

So I started looking at other text and did ROT47 on that and it tourned out to be base64 `e1o6A1qXqFvaG77XpHZTxOcnepxXYIJlGj0GqpLXltQ=`

Used this as a key and decoded that cipher 

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/fernet_decoded.JPG)

we got some base64
 ```
 JiMxMTA7JiMxMTE7JiMxMTE7JiM5ODsmIzEyMzsmIzEwMjsmIzEwMTsmIzExNDsmIzExMDsmIzUxOyYjMTE2OyYjOTU7JiM5ODsmIzk3OyYjOTg7JiM1MTsmIzk1OyYjMTIxOyYjMTExOyYjMTE3OyYjOTU7JiM5NzsmIzExNDsmIzUxOyYjOTU7JiMxMDg7JiM1MTsmIzUxOyYjMTE2OyYjMTI1Ow==
 ```
After decoding it we get html entity, so after decoding it 

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/whatever.JPG)

we get the flag `noob{fern3t_bab3_you_ar3_l33t}`

