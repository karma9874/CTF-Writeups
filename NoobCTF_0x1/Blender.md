# Challenge Title: Blender

## Description
The pr03ssor is back with a new MISSION!

Link : http://159.89.163.214:5959

## Solution

Here we have a link to which runs a clip from money hesit, nothing much in source code 

cheked `/robots.txt` and got this

```
User-agent: *
Disallow: /flag.html
```

got this  `/flag.html`

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/blunder_robots.JPG)

So the clue says `Check out author's margatsnI`

the reverse of `margatsnI` is `Instagram`. So from author instagram i got this post with comment

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/dev_lead.JPG)

Rot13 of `uggcf://jjj.fubeggb.pbz/eri01-gkg` will give a url `https://www.shortto.com/rev01-txt`

From the url we get a txt file which has hex value in it. So i download that file and decoded the hex value and save it in file turns out it is a jpeg file and we get the image of proffesor 

`cat noob\ \(2\).txt | xxd -r -p > data `

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/prof.JPG)


So i used stegsolve on that image and started changing channel and i got this data in one of the channel `H3ispr0f3ssorn00b`

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/stegsolve.JPG)

used that data to stegide and got some hex encoded file again

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/steghide.JPG)

i tried to decode the hex but it was all rubbish

so i tried to reverse the hex and then decode and it turns out to be png image but it was broken

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/broken_iamge.JPG)

so edited the magic bytes of png to the file using hexeditor

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/magic_bytes.JPG)

so after opening the image we got the flag `noob{pr0f3ssor_1s_l33t}` which turns out to a fake one :(

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/fake_flag.JPG)

so remeber the page from `/flag.html` we need to submit the fake flag there to get real flag

so we get the flag from there `noob{y0u_w0rk3d_h4rd_f0r_pr0f3ssor_s0_y0u_4re_IN}`

![alt text](https://github.com/karma9874/CTF-Writeups/blob/master/NoobCTF_0x1/Images/flag.JPG)
