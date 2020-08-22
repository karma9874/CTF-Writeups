# Hastad's message
## Crypto

### Description:
> My friend Hastad send me a message but I am not able to read it. Only thing I know is its different each time.
>nc crypto.zh3r0.ml 7419

>Author : Finch

### Overview:
Connecting using the given netcat command gave us this:
```
$ nc crypto.zh3r0.ml 7419
N: 27824871573635346100575105383717622949311054878538662109901612154980317487418558197528376952593144490007815316755380941913723765539053284766828021084784434328703737362755252606758417852695150717362961926742444633345120466471542035052172891810514125398164700298118728678170741869964320679655745315390571555650015501737537332951181778410193296977590334228554660299573159978781400246564112138995928670487959397311566195900179648855587660704151028117070191293169044664389068733765909613625669190983898755349699834295596036746813762849222086253412802305182971409553145019727063076397480103751749743813673993796322283318581
CT: 11019000074973310505868058261793650884324300588016752108729987114642205921089871737723394812557846490278473944808838531739350934013522016565993683946475326937088282601710158221520273084850498390425468986213946601794117125452441635818648605091580291730894237814492080440128646112199382337855224545448282317782267618771021508162322357418987500749236337726466626322308649756446345704650142544790397240756356415044495188481601640656595190729573322163931589396898774661152658781941340458521067120732816745459928250131251782303174038799370270843718318743618633232378296798201078556894515676762121216917661342583576341015153
```

### Solution:

So, as the challenge name sugguest that it should be realated to `Hastad Method`, which means here the value of e is small. Upon connecting to the service everytime we get different value of `N` and `CT` so we can perform `Coppersmith's Attack`: collect a list of 3 `N` and `CT` distinct value and by using the below code with all the requirments we get the flag. I tried changing the `e` value from 3 to 10 and I got the flag at `e=5`

##### Sample Python2 Code:
```
e = 5
n1 = <n1>
n2 = <n2>
n3 = <n3>
c1 = <c2>
c2 = <c1>
c3 = <c3>

def chinese_remainder(n, a):
    sum = 0
    prod = reduce(lambda a, b: a*b, n)
    for n_i, a_i in zip(n, a):
        p = prod / n_i
        sum += a_i * mul_inv(p, n_i) * p
    return sum % prod

def mul_inv(a, b):
    b0 = b
    x0, x1 = 0, 1
    if b == 1: return 1
    while a > 1:
        q = a / b
        a, b = b, a%b
        x0, x1 = x1 - q * x0, x0
    if x1 < 0: x1 += b0
    return x1

def find_invpow(x,n):
    high = 1
    while high ** n < x:
        high *= 2
    low = high/2
    while low < high:
        mid = (low + high) // 2
        if low < mid and mid**n < x:
            low = mid
        elif high > mid and mid**n > x:
            high = mid
        else:
            return mid
    return mid + 1

flag_cubed=chinese_remainder([n1,n2,n3],[c1,c2,c3])


flag=find_invpow(flag_cubed,e)
print "flag: ",hex(flag)[2:-1].decode("hex")
```
##### Output:
```
flag:  Its great that you have come till here. As promised here is your flag: zh3r0{RSA_1s_0n3_of_th3_b4st_encrypt10n_Bu66y}
```


### Flag:
>zh3r0{RSA_1s_0n3_of_th3_b4st_encrypt10n_Bu66y}
