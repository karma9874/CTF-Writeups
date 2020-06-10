# Challenge Title: Advance encryption? 

## Description
Hey I have leaked one convo between the author and some anonnymous find out what flag they were talking about.

Given file [leaked.txt](https://raw.githubusercontent.com/The-deviner/NoobCTF-0x1/master/leaked.txt)

## Solution

From the file we know it is rsa and we have n,p,c

We get the fator from [factor db](http://factordb.com/)

```
p =  37811 
q = 137894708910053303969923397973020642822885800296469378790977278593214384539837619580379225467154996305576740301811341001236212352803012852463937630519740268712559979364296984889908082141326523753012648585274606560093642698194606727748070085995321631608004390601327049361234087266324892040367357035145489719811
```

Used this script to decode ct
```
p =  37811 
q = 137894708910053303969923397973020642822885800296469378790977278593214384539837619580379225467154996305576740301811341001236212352803012852463937630519740268712559979364296984889908082141326523753012648585274606560093642698194606727748070085995321631608004390601327049361234087266324892040367357035145489719811
c = 1318662676012529027719356593897795240255894626324734057679095070946627050031960058953695686381615923039181791966536008755483122090144422057413223859758218760258478899092958126133112186690655374761969096836593572888239450083333192693873889204362146378414307476977579570911929579923802717779725077182959279149667910
n = p*q
e = 65537
from Crypto.Util.number import inverse
phi = (p-1)*(q-1)
d = inverse(e,phi)
m = pow(c,d,n)
a= hex(m)[2:-1].decode('hex')
print(a)
```

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/rsa.JPG)

And we get 
```
7A392A1577F7921D840A5DD8BC5C2C184DE17387E68D6168,b15bfdaa5c0e3a1ae9d2b435cdee81eba9e037d99bae6fb7f79bb00a6e1903fb
```

Now this is where I stucked, from description we need to find algo wich is more secure than des and less secure than aes 

So after lot of reseaching i got TripleDES algo 

Triple DES needs key,iv to decode hmmm..

So i tried first part from rsa output as key and second output as input separated by comma

Now what about IV ?

I changed the mode to EBC which doesnt requires IV

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/des.JPG)

and we get the flag `noob{3des_1s_a_g00d_encrYpt1oN}`
