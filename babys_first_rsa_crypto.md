## Challenge: Baby's First RSA [Crypto] 150 Points

I just learned about RSA and I am pretty sure that I implemented it right. It should be impossible to get my flag.

Two files were provided:  main.py and out.txt

```
└─$ cat main.py         
from Crypto.Util.number import bytes_to_long, getPrime
from secret import FLAG

def genRSA():

  # public exponent set to 3, a common choice for RSA.  Other choice is 65535 <- check this number
  e = 3
  while True:
    # generate p and q until phi is not divisible by e
    p = getPrime(512)
    q = getPrime(512)

    # Euler totient function
    phi = (p-1)*(q-1)
    if phi % e:
      break

  # RSA modulus
  n = p*q

  # private exponent
  d = pow(e,-1,phi)

  return (e,n,d)
  
def main():
  e,n,d = genRSA()

  # encrypted flag obtained by raising the long integer of FLAG to
  # the power of e modulo n
  enc_flag = pow(bytes_to_long(FLAG),e,n)

  print("Here is your encrypted flag: " + str(enc_flag))
  print("Here is your public key:")
  print("n: " + str(n))
  print("e: " + str(e))

if __name__ == '__main__': main()
```
```
$ cat out.txt
Here is your encrypted flag: 11320887921865707970417131707489304941213737344372772560296232001708703523599042195968223212365109776754039820465372975539526543057079098227551678593290445701559045011482149948708333749562432591623529530280037
Here is your public key:
n: 54635592099855565238567429816156089377033822002759547411082764468188951140701492941799814994802894116863539008046955775901349438057474600774506026999322449088884781059206427090047834145264757894872328436141156254487939678497662258017309980269148722038770041654103035346970408674206071958598445348607191506511
e: 3
```

## Solution
### Step 1: Review main.py
main.py includes an import statement: 
```
from secret import FLAG
```
I used a text editor to create a dummy FLAG and stored it in secret.py
```
└─$ cat secret.py  
FLAG = b'dummyFLAG'
```
### Step 2: Write a Python script
I did some research and chose to install the [cryptography](https://pypi.org/project/cryptography/) module.  Then, I copied main.py to a new script called, rsa_calcs.py.  I had a chat with Copilot and came up with this script:
```
import rsa
from Crypto.Util.number import long_to_bytes, getPrime, bytes_to_long
from secret import FLAG

def genRSA():
  # public exponent set to 3, a common choice for RSA.  Other choice is 65537
  e = 3
  while True:
    # generate p and q until phi is not divisible by e
    p = getPrime(512)
    q = getPrime(512)

    # Euler totient function
    phi = (p-1)*(q-1)
    if phi % e:
      break

  # RSA modulus
  n = p*q

  # private exponent
  d = pow(e,-1,phi)

  return (e,n,d,p,q)

def main():
    e, n, d, p, q = genRSA()

    # Because I have both public and private keys, I can decode the encrypted message in secret.py or
    I can substitute the encrypted FLAG from out.txt (in string format)
    
    enc_flag = pow(bytes_to_long(FLAG), e, n)
    print(type(enc_flag))

    #####################################################################
    # I added this:
    # Since the script has both a private and public key, it can decrypt
    # a flag by substituting the ciphertext of the flag
    enc_flag = 11320887921865707970417131707489304941213737344372772560296232001708703523599042195968223212365109776754039820465372975539526543057079098227551678593290445701559045011482149948708333749562432591623529530280037
    #####################################################################

    print("Encrypted flag: " + str(enc_flag))
    print("")
    print("private_key.pem")
    print("public_key.pem")

    # To save the private key in PEM format
    private_key = rsa.PrivateKey(n, e, d, p, q)
    with open('private_key.pem', 'wb') as f:
        f.write(private_key.save_pkcs1('PEM'))

    # To save the public key in PEM format
    public_key = rsa.PublicKey(n, e)
    with open('public_key.pem', 'wb') as f:
        f.write(public_key.save_pkcs1('PEM'))

    # Decrypting the encrypted flag
    dec_flag = pow(enc_flag, d, n)
    print("")
    print("Here is your decrypted flag: " + long_to_bytes(dec_flag).decode())

if __name__ == '__main__':
    main()
```
Here's the output from rsa_decoder.py
```
$ python3 rsa_decoder.py                           

Encrypted flag: 11320887921865707970417131707489304941213737344372772560296232001708703523599042195968223212365109776754039820465372975539526543057079098227551678593290445701559045011482149948708333749562432591623529530280037

private_key.pem
public_key.pem

Here is your decrypted flag:
SIVBGR{D0nt_F0rg37_T0_P4D!!!}
```

### Cube root
Copilot provided an explanation of the math along with examples.  After the event, Jacob Elliott offered a coupon for his Udemy course, CTF 101: Competitive Learning in Cybersecurity.  Rather than validate the Copilot explanation, I thought I would learn faster by signing up for this class.  I found a more elegant solution in the Cryptography module along with an explanation of the math that was easy to understand.

```
from Crypto.Util.number import long_to_bytes, getPrime, bytes_to_long

def cube_root(n):
    low = 0
    mid = n//2
    high = n

    while low < mid < high:
        if mid **3>n:
            high = mid -1
        elif mid ** 3 < n:
            low = mid + 1
        mid = low + ((high-low)//2)
    return mid


print("Expecting out.txt to contain")
print("Here is your encrypted flag: 1132...")
print("Here is your public key:")
print("n: 546...")
print("e: 3\n")

filename = 'out.txt'
with open(filename, 'r') as f:
    data = f.read()

# given the format of out.txt shown above
lines = data.split(": ")
f = lines[1].split("\n")
Flag = int(f[0])
nn = lines[2].split("\n")
n = int(nn[0])
e = 3

print(long_to_bytes(cube_root(Flag)))
```
```
$ python3 cube_root.py                             
Expecting out.txt to contain
Here is your encrypted flag: 1132...
Here is your public key:
n: 546...
e: 3

b'SIVBGR{D0nt_F0rg37_T0_P4D!!!}'
```
