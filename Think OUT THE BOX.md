# Challenge Title: Think OUT THE BOX

## Description
Guys we always underestimate the length of our cipher. But it will not help you further if you continue it ...

Given file [Think_OUT_THE_BOX.txt](https://raw.githubusercontent.com/AdityaSec/NoobCTF-0x1/master/Misc/Think%20OUT%20THE%20BOX/Think_OUT_THE_BOX.txt)

## Solution

After reading the description, it seems to do something with length and the text in that file seems to be comma separated.

So my teamate said what if we count the lenght of the string till comma, so it trurns out that the length was ascii representation of flag

I wrote a quick python script to do that

``` 
[print(chr(len(i)),end="") for i in open("Think_OUT_THE_BOX.txt").read().strip().split(",")]
```

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/think_decoded.JPG)

we got the flag `noob{Leng7h_1s_pr3c10u$_dfrgytncgsnsdsdl!!!}`