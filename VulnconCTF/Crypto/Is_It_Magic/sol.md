## Is_it_magic


We are provided with an encrypted image(jpeg) file. The encryption is done by XORing a secret key with the hexdump of the file.
Our task is to find the secret key and decrypt the image back to get the flag.

Magic bytes of a JPEG image file : `FF D8 FF E0 00 10 4A 46 49 46 00 01` >  XOR the first 12 chunks of given file and magic bytes og JPEG to get the key : `46ccf9a571f0ffb17e41cb84`
> Key-XOR it with the given file in a cyclic manner to retrieve the original image

```
Flag : vulncon{x0r_1s_n0t_s@f3}
```
