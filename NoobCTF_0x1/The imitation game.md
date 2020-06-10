# Challenge Title: The imitation game

## Description
Have you watched this movie??

Here two files are given [game.txt](https://raw.githubusercontent.com/AdityaSec/NoobCTF-0x1/master/Misc/The%20imitation%20game/game.txt) and pimmitation.pdf](https://github.com/AdityaSec/NoobCTF-0x1/raw/master/Misc/The%20imitation%20game/immitation.pdf)

## Solution
The `game.txt` has something encoded,seems to be somekind of substituition cipher or enigma, after trying to decode the text with `quipquip.com` I got nothing so lets move to the next file for now.

The pdf file is password protected so `fcrack` can help us

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/imitation_fcrack.JPG "fcrack res")

So we got password `alejandro` lets open the pdf with this creds

Ohh the pdf has nothing than just `I see nothing Here.`

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/imitation_pdf.JPG "Nothing")

Just select all the text on the file (ctrl+A)

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/imitation_hidden.JPG)

There is something on the file but hidden

So I just copy all the test and pasted on a text editor and I got this

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/imitation_token.JPG)

After reasearching a bit I got to know that this is enigma rotors value. So lets try to decode that text from the `game.txt`

I used this [Cryptii engima decoder](https://cryptii.com/pipes/enigma-decoder) tool to decode the enigma

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/imitatoin_decoded.JPG)

we got the flag `noob{are_you_paying_attention}`