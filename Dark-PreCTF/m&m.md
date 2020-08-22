# Challenge Title: m&m

## Description
You know your Morse Code?
A variant of fractioned morse code btw
PS: Flag is in full caps separated by underscore

Flag format: darkCTF{FLAG}

[Link](https://mega.nz/file/2gFi0CbT#w5Zv3D5ui1jhdjTLYgfUO9I7ImZ8rVMWRn32NtVm-Ig)

## Solution

This chall was [morbit cipher](https://www.dcode.fr/morbit-cipher)

After decoding the morse code from the text file we get 

```
7651785856247823325646643238331784648
```
But morbit cipher needs key to decode it, well we can bruteforce it because morbit uses (0-9) numbers to as a key

Here is my script to bruteforce it 
```
decoded_morse = "7651785856247823325646643238331784648"

from itertools import permutations

MORSE_CODE_DICT = {'..-': 'U', '--..--': ', ', '....-': '4', '.....': '5', '-...': 'B', '-..-': 'X', '.-.': 'R', '--.-': 'Q', '--..': 'Z', '.--': 'W', '-..-.': '/', '..---': '2', '.-': 'A', '..': 'I', '-.-.': 'C', '..-.': 'F', '---': 'O', '-.--': 'Y', '-': 'T', '.': 'E', '.-..': 'L', '...': 'S', '-.--.-': ')', '..--..': '?', '.----': '1', '-----': '0', '-.-': 'K', '-..': 'D', '----.': '9', '-....': '6', '.---': 'J', '.--.': 'P', '.-.-.-': '.', '-.--.': '(', '--': 'M', '-.': 'N', '....': 'H', '---..': '8', '...-': 'V', '--...': '7', '--.': 'G', '...--': '3', '-....-': '-', '\n' : ' '}
MORBIT = ['..', '.-', '. ', '-.', '--', '- ', ' .', ' -', '  ']

def morse(ciphertxt,flag=None): 
    plaintxt = ''
    for word in ciphertxt.strip().split("  "):
        for c in word.strip().split(" "):
            if c in MORSE_CODE_DICT:
                plaintxt += MORSE_CODE_DICT[c]
            else:
                pass
        plaintxt += ' '
    return plaintxt
    
def morbit(ciphertxt,flag=None):
    if flag==None:
        flag=""
    scores=[]
    for p in permutations('0123456789'):
        MORBIT_CODE_DICT = dict(zip(p, MORBIT))
        morsetxt = ""
        for c in ciphertxt:
            if c in MORBIT_CODE_DICT:
                morsetxt += MORBIT_CODE_DICT[c]
        if flag in morse(morsetxt,flag):    
        	print(morse(morsetxt,flag))
        
morbit(decoded_morse,"DARKCTF")
```

Running this script will give you the flag (key = 555542069)

Flag -> darkCTF{MORSE_2_MORBIT}