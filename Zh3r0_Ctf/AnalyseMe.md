# Analyse me

## Crypto

### Description:
>i have a server's ip which gives us the flag, but i'm not so good in cracking it. Can you decode the flag for me?
>nc crypto.zh3r0.ml 3871

>Author : Whit3_D3vi1

### Solution:

Just a basic python code reversing

### Code (Python3):

```
from Crypto.Util.number import *
from Crypto.Util.strxor import strxor
from binascii import *
from base64 import *
from pwn import *


def from_the_bases(msg,count):
    count=(count%4)+1
    if count == 1:
        return b64decode(msg)
    elif count == 2:
        return b32decode(msg)
    elif count == 3 :
        return b85decode(msg)
    else:
        return unhexlify(msg)

p = remote("crypto.zh3r0.ml",3871)

p.recvuntil("Here is the first part:")
for i in range(3):
    p.recvline()

first_part = p.recvline().decode("utf-8")
first_part = first_part[:-2].split("|")

count=0
fp_decode=""
for i in first_part:
    fp_decode+=from_the_bases(long_to_bytes(int(i)),count).decode('utf-8')
    count+=1

p.sendlineafter("message:",fp_decode)

p.recvline()
key = p.recvline().decode('utf-8').split(":")[1][2:]
key = long_to_bytes(key).decode('utf-8')
print("Key is : "+key,"\n")
p.recvuntil(b'GOOD LUCK DECODING!!! \n')
p.recvline()
xor_list = p.recvline().decode("utf-8")[1:-1]

xor_list = ['\x07\x01WQUT\x01\x00', 'R\x07\x07VWUW\x0f', '\x05RQ\x01\x01R\x02\x05', 'W\x01P\n\x05SU\x06', 'V\x02VQ\x04V\x06\x01', 'U\x07P\x0fR\x04TQ', '\x05XU\x06\x07\x07\x03\x02', '\x03R\x03P\x02\x06\x02U', 'UR\x04PU\x0fV\x04']

print("Given XOR List : ",xor_list)

final_key = key
key_list = [hexlify(final_key[i:i+4].encode()).decode() for i in range(0,len(key),4)]

# print(key_list)

flag_list=[strxor(i.encode(),j.encode()) for i,j in zip(xor_list,key_list)]

# print(flag_list)

table_reversed={'63': '00', '7c': '01', '77': '02', '7b': '03', 'f2': '04', '6b': '05', '6f': '06', 'c5': '07', '30': '08', '01': '09', '67': '0a', '2b': '0b', 'fe': '0c', 'd7': '0d', 'ab': '0e', '76': '0f', 'ca': '10', '82': '11', 'c9': '12', '7d': '13', 'fa': '14', '59': '15', '47': '16', 'f0': '17', 'ad': '18', 'd4': '19', 'a2': '1a', 'af': '1b', '9c': '1c', 'a4': '1d', '72': '1e', 'c0': '1f', 'b7': '20', 'fd': '21', '93': '22', '26': '23', '36': 'f5', '3f': '25', 'f7': '26', 'cc': '27', '34': '28', 'a5': '29', 'e5': '2a', 'f1': '2b', '71': '2c', 'd8': '2d', '31': '2e', '15': '2f', '04': '30', 'c7': '31', '23': '32', 'c3': '33', '18': '34', '96': '35', '05': '36', '9a': '37', '07': '38', '12': '39', '80': '3a', 'e2': '3b', 'eb': '3c', '27': '3d', 'b2': '3e', '75': '3f', '09': '40', '83': '41', '2c': '42', '1a': '43', '1b': '44', '6e': '45', '5a': '46', 'a0': '47', '52': '48', '3b': '49', 'd6': '4a', 'b3': '4b', '29': '4c', 'e3': '4d', '2f': '4e', '84': '4f', '53': '50', 'd1': '51', '00': '52', 'ed': '53', '20': '54', 'fc': '55', 'b1': '56', '5b': '57', '6a': '58', 'cb': '59', 'be': '5a', '39': '5b', '4a': '5c', '4c': '5d', '58': '5e', 'cf': '5f', 'd0': '60', 'ef': '61', 'aa': '62', 'fb': '63', '43': '64', '4d': '65', '33': '66', '85': '67', '45': '68', 'f9': '69', '02': '6a', '7f': '6b', '50': '6c', '3c': '6d', '9f': '6e', 'a8': '6f', '51': '70', 'a3': '71', '40': '72', '8f': '73', '92': '74', '9d': '75', '38': '76', 'f5': '77', 'bc': '78', 'b6': '79', 'da': '7a', '21': '7b', '10': '7c', 'ff': '7d', 'f3': '7e', 'd2': '7f', 'cd': '80', '0c': '81', '13': '82', 'ec': '83', '5f': '84', '97': '85', '44': '86', '17': '87', 'c4': '88', 'a7': '89', '7e': '8a', '3d': '8b', '64': '8c', '5d': '8d', '19': '8e', '73': '8f', '60': '90', '81': '91', '4f': '92', 'dc': '93', '22': '94', '2a': '95', '90': '96', '88': '97', '46': '98', 'ee': '99', 'b8': '9a', '14': '9b', 'de': '9c', '5e': '9d', '0b': '9e', 'db': '9f', 'e0': 'a0', '32': 'a1', '3a': 'a2', '0a': 'a3', '49': 'a4', '06': 'a5', '24': 'a6', '5c': 'a7', 'c2': 'a8', 'd3': 'a9', 'ac': 'aa', '62': 'ab', '91': 'ac', '95': 'ad', 'e4': 'ae', '79': 'af', 'e7': 'b0', 'c8': 'b1', '37': 'b2', '6d': 'b3', '8d': 'b4', 'd5': 'b5', '4e': 'b6', 'a9': 'b7', '6c': 'b8', '56': 'b9', 'f4': 'ba', 'ea': 'bb', '65': 'bc', '7a': 'bd', 'ae': 'be', '08': 'bf', 'ba': 'c0', '78': 'c1', '25': 'c2', '2e': 'c3', '1c': 'c4', 'a6': 'c5', 'b4': 'c6', 'c6': 'c7', 'e8': 'c8', 'dd': 'c9', '74': 'ca', '1f': 'cb', '4b': 'cc', 'bd': 'cd', '8b': 'ce', '8a': 'cf', '70': 'd0', '3e': 'd1', 'b5': 'd2', '66': 'd3', '48': 'd4', '03': 'd5', 'f6': 'd6', '0e': 'd7', '61': 'd8', '35': 'd9', '57': 'da', 'b9': 'db', '86': 'dc', 'c1': 'dd', '1d': 'de', '9e': 'df', 'e1': 'e0', 'f8': 'e1', '98': 'e2', '11': 'e3', '69': 'e4', 'd9': 'e5', '8e': 'e6', '94': 'e7', '9b': 'e8', '1e': 'e9', '87': 'ea', 'e9': 'eb', 'ce': 'ec', '55': 'ed', '28': 'ee', 'df': 'ef', '8c': 'f0', 'a1': 'f1', '89': 'f2', '0d': 'f3', 'bf': 'f4', '42': 'f6', '68': 'f7', '41': 'f8', '99': 'f9', '2d': 'fa', '0f': 'fb', 'b0': 'fc', '54': 'fd', 'bb': 'fe', '16': 'ff'}


fin=[]
for j in range(2,5):
    for i in flag_list:
        char1=i.decode()
        num=0
        while num<j:
            char1=table_reversed[char1[0]+char1[1]]+table_reversed[char1[2]+char1[3]]+table_reversed[char1[4]+char1[5]]+table_reversed[char1[6]+char1[7]]
            num+=1
        fin.append(char1)
    print("\n")
    print(unhexlify(''.join(fin)))
    fin=[]
```