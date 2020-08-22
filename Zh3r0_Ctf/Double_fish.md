# Double fish
## Crypto

### Description:
>There are two fishes in the Xander's Operative Register
>A flag is waiting for you when you catch both.
>_0m\K2!2%\ggrdups\vd~gq

>Author : Finch 

### Solution:

So here the the name of challenge desciption says ` Xander's Operative Register`  XOR so I tried xoring the key on Cyberchef with length two and got something `K3y__1511_sdfgasg_bgjde` at key 1403. 

Here the chall.txt contains repetition of letters like `x,k,c,d` after a bit of searching and with the help of team I got to know that it is a `deadfish esolang`. I used this [decoder](https://www.dcode.fr/deadfish-language) and got the decoded text `pFvkylIBH33Qlu0t7rgPk98SrYGz5kt1pKe+2lCKxZ0=`. So now we have a encrypted text and a key

As the chall name is Double Fish I thought about `Two Fish` encryption to decode the rest of the part but it didnt worked. After this I tried [Blow fish](https://codebeautify.org/encrypt-decrypt) with CBC mode and it worked and I got the flag 

### Flag: 
>zh3r0{B10w_7h3_Fish_!!}