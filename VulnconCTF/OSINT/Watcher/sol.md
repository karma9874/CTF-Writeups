## The Watcher

![chall](https://github.com/RDxR10/CTF-Writeups-1/tree/master/VulnconCTF/OSINT/Watcher)

- The challenge description tells us there is user named *tim3zapper* and he wants to us to find the email id of a photographer.
- tim3zapper > twitter profile. Going over his tweets, we observe that one of the tweets were deleted but backed up somewhere > Guesswork > internet archive
- Web Archives > a user mentioned `@sullyth3h4x0r`.

- Sherlock tool > Info based on ello page with this username [https://ello.co/sullyth3h4x0r](https://). In the page, we see a picture of the plane and a caption > to find the photographer

- Now we know that the mail id in the flag corresponds to the photographer who took this photo
![](https://i.imgur.com/Qyirf12.jpg)

- Reverse image search on google > a page [https://www.jetphotos.com/registration/N251HR](https://) with the photographer's name > `Agustin Anaya`

- Search Agustin Anaya on google > [http://www.dutchops.com/Result.asp?PageNo=1&Soort=%&Airline=Delta%20Airlines](https://) > a contact email button. 
- We get the email address which is our flag.

```
Flag- vulncon{m.venema@dutchops.com}
```
