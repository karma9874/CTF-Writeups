# Challenge Title: Pickle Image

## Description
Someone said this file is related to something pickle image or pickle's image or pickle 
and image idk can you find the flag?

Flag format: darkCTF{FLAG}

[Link](https://mega.nz/file/fxcHnA5b#wDs4MVWxCZKCddprniFp32lVBmrU8vD5PiQp0_66yks)

## Solution

The given file was a python's pickle dump, on loading the dump to pickle you can see that it is 3d array which states for RGB (by desciption)

Heres the script

```
import pickle
from PIL import Image
from numpy import asarray

a = pickle.load(open("weird",'rb'))
img = Image.fromarray(a, 'RGB')


img.show()
```