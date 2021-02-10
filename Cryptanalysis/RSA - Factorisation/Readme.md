## Statement
The validation password was encrypted using this public key

ciphertext: e8oQDihsmkvjT3sZe+EE8lwNvBEsFegYF6+OOFOiR6gMtMZxxba/bIgLUD8pV3yEf0gOOfHuB5bC3vQmo7bE4PcIKfpFGZBA

And they gave a zip file which had the public key
```
-----BEGIN PUBLIC KEY-----
MGQwDQYJKoZIhvcNAQEBBQADUwAwUAJJAMLLsk/b+SO2Emjj8Ro4lt5FdLO6WHMM
vWUpOIZOIiPu63BKF8/QjRa0aJGmFHR1mTnG5Jqv5/JZVUjHTB1/uNJM0VyyO0zQ
owIDAQAB
-----END PUBLIC KEY-----
```

So I wrote a pyhton script for the  solution

```
#!/usr/bin/env python3

from Crypto.PublicKey import RSA
from base64 import *
from Crypto.Util.number import inverse, long_to_bytes

with open('pubkey.pem', 'r') as f:
	key = RSA.importKey(f.read())

n = key.n
e = key.e
en = b64decode("e8oQDihsmkvjT3sZe+EE8lwNvBEsFegYF6+OOFOiR6gMtMZxxba/bIgLUD8pV3yEf0gOOfHuB5bC3vQmo7bE4PcIKfpFGZBA")
c = int.from_bytes(en, byteorder='big', signed=False)
p = 398075086424064937397125500550386491199064362342526708406385189575946388957261768583317
q = 472772146107435302536223071973048224632914695302097116459852171130520711256363590397527

phi = (p-1)*(q-1)
d = inverse(e,phi)
m = pow(c,d,n)
print(str(long_to_bytes(m)[56:-1]))
```
The flag contains lot of padding so i carved only the readable text

## Flag
b'up2l6DnaIhZgxA'
