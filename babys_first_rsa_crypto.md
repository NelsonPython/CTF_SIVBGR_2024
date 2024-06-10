## Challenge: Baby's First RSA [Crypto] 150 Points

I just learned about RSA and I am pretty sure that I implemented it right. It should be impossible to get my flag.

Two files were provided:  main.py and challenge

## Solution
### Step 1: View main.py
main.py includes an import statement: 
```
from secret import FLAG
```
I used a text editor to creat a dummy FLAG and stored it in secret.py
```
└─$ cat secret.py  
FLAG = b'dummyFLAG'
```
### Step 2: Write a Python script
I did some research and chose to install the [cryptography](https://pypi.org/project/cryptography/) module.  Then, I copied main.py to a new script called, rsa_calcs.py.  I had previously received permission to use AI so I had a chat with Copilot.  Here's the script written by Copilot with a few modifications:
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

## Notes from Copilot
### Me:  
explain the pow statement in python
### Copilot
In Python, the pow function is often used in RSA key generation to perform modular exponentiation. The pow function can take two or three arguments: 
pow(x, y) computes 𝑥𝑦 = xy
pow(x, y, z) computes (x) raised to the power (y), modulo (z)

In the context of RSA key generation, this function is crucial for calculating the public and private keys. The RSA algorithm relies on the difficulty of factoring large prime numbers, and the pow function is used to raise a number to a power within the modulus of these large primes, which is a part of the encryption and decryption process.

