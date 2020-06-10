# Challenge Title: CrackMeh

## Description
3lli0t found a diary from Evil Corp. He have to get into the system, but can't as he have a hash of a password and no plaintext. Help him to get into system.

flag format: noob{plaintext}

hash: 4ee805f9397a1d584ef9be9d2a4f8f20

Here in the given file we have some text 
```
_________________________
|			            |
|			            |	
|			            |
|	  Alice		        |
|	 January	        |
|	  1994		        |
|      USA		        |
|	   25		        |
|    Security	        |
|			            |
|			            |
|_______________________|
```
## Solution

The given hash is md5 , I tried to crack the hash with john with the given text but no luck

So i created a wordlist from all given words
```
a=["Alice","January","1994","USA","25","Security"]
import itertools
for j in range(6):
	for i in itertools.permutations(a,j):
		print(''.join(i))
```

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/wordlist.JPG)

So we have `1237` possible passwords lets try to crack with this list

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/md5_cracked.JPG)

we get the flag `noob{AliceSecurity1994}`

