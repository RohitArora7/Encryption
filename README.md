# Encryption

A 128-bit keys are sufficient to ensure a good security, but is up to you to choose an AES192 or AES256.


AES ECB and CBC mode

ECB mode - cannot send the intialization Vector(IV).\
CBC mode -  we just have one diference the IV ( Initialization Vector) which should be 128 bits pr 16 char too. We can use a fix IV or random IV. That’s up to you the random is more secure both should be passed to the Javascript for decryption.

```bash
#FIX IV
iv =  'BBBBBBBBBBBBBBBB'.encode('utf-8') #16 char for AES128 #b'BBBBBBBBBBBBBBBB'
```

```bash
#Random IV more secure
iv =  get_random_bytes(16) #16 char for AES128
```

 node uses OpenSSL which uses PKCS5 to do padding. PyCrypto doesn't handle the padding so I was doing it myself just add ' ' in both.
 
 
 ```bash
 There's a small bug in your Node.js decrypt function. It won't handle multiple - or multiple /. Also, in decrypt, you need to replace _ with /, not the other way around. You can simply replace that line with: var input = input.replace(/\-/g, '+').replace(/_/g, '/'); 
 ```
 
 
 
 You use a random IV to encrypt some plaintext in Python. If you want to retrieve that plaintext, you need to use the same IV during decryption. The plaintext cannot be recovered without the IV. Usually the IV is simply prepended to the ciphertext, because it doesn't have to be secret. So you need to read the IV during decryption and not generate a new one.

You use CBC mode in CryptoJS (default) instead of CFB mode. The mode has to be the same. The other tricky part is that CFB mode is parametrized with a segment size. PyCrypto uses by default 8-bit segments (CFB8), but CryptoJS is only implemented for fixed segments of 128-bit (CFB128). Since the PyCrypto version is variable, you need to change that.

The CryptoJS decrypt() function expects as ciphertext either an OpenSSL formatted string or a CipherParams object. Since you don't have an OpenSSL formatted string, you have to convert the ciphertext into an object.

The key for CryptoJS is expected to be a WordArray and not a string.

Use the same padding. PyCrypto doesn't pad the plaintext if CFB8 is used, but padding is needed when CFB128 is used. CryptoJS uses PKCS#7 padding by default, so you only need to implement that padding in python.
 

