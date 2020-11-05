# Challenge Title: Flag Checker

## Description
Reverse to get the flag

Flag format: darkCTF{FLAG}

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