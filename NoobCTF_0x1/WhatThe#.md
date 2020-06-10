# Challenge Title: WhatThe#

## Description
My brain can't interpret this, can you?

Given file [chall1.txt](https://raw.githubusercontent.com/AdityaSec/NoobCTF-0x1/master/Crypto/WhatThe%23/chall1.txt)
## Solution

From text file we see this brainfuck esolang
```
----------]<-<---<-------<---------->>>>+[<<<<----------,-,,+++++++++++++,-------------------------,>--------,>------------------,<<+++++++,>-----------------,>----,<<++++++++,-----------,>>,<<--,>>-,<,---,<+++++++,>>+,+++,<<++++,---------------,
```
First I tried to decode the this with `dcode fr` but it gave me some errors

So after some researching I got to know there is other kind of brainfuck to know as `reversefuck`

Used this [decoder](https://www.dcode.fr/reversefuck-language) 

and we get the flag `noob{N0t_4lw4y5_br41n}`


