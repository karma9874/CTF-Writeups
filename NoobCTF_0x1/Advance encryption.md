# Challenge Title: Frequency

## Description
Elliot captured something, while noob called his friend

Flag format: noob{FLAG}

Given file [Cipher.txt](https://raw.githubusercontent.com/AdityaSec/NoobCTF-0x1/master/Crypto/Frequency/Cipher.txt)

## Solution

Here we have 
```
1209-770 1209-770 1477-697 1477-697 1336-770 1336-770 1336-770 1336-770 1336-770 1336-770 1477-770 1477-770 1477-770 1477-697 1336-852 1477-770 1477-697 1477-697 1477-697
```

Which I know is DTMF code so I used this (decoder)[https://www.dcode.fr/dtmf-code] which gave me 

```
4433555555666386333
```

And this is Multi-tap Phone Cipher decode using this [decoder](https://www.dcode.fr/multitap-abc-cipher)

and we get the flag `noob{HELLODTMF}`


