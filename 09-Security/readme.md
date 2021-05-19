# 09 - Security and Cryptography

## Entropy

Entropy is measured in bits - measures amount of randomness
Entropy = log2 (number of possibilities)

coin flip = log2 (2) = 1
dice = log2 (6) = 2.6

## Hash Functions

Map a variable amount of data into a fixed output
SHA1 takes an input and converts it to 160 bits

Hard to invert
`printf 'hello' | sha1sum` returns a 40 length hexadecimal string

#### Hash Function Properties

1. Non-invertable - can't reverse engineer the input from output
2. Collision resistent - hard to find 2 inputs that produce the same output
3. Deterministic - same input will always get same output

#### To ensure right repo

We can download a software from source, and another from a mirror.
Then we compare their hash.

#### Binding Commitment

To avoid cheating, we can write down our decison and provide the SHA1 hash to the other person.

#### Key Derivation Functions

Same as hash function, but is slow to compute
`PBKDF2`
But the purpose is to use this output in another crypto aglo.
These are slow, to hinder brute force password attacks.

## Symmetric Key Cryptography

Key() function to generate a key
encrypt() - converts plaintext and a key => produce ciphertext
decrypt() - converts ciphertext and a key => produce plaintext

-   Given ciphertext, can't figure out plaintext

Use case: Encrypting files for storage on untrusted cloud service.

#### OpenSSL

**To Encrypt:**
`openssl aes-256-cbc -salt -in README.md -out README.enc.md`
Then enter passphrase
`cat README.enc.md`

**To Decrypt:**
`openssl aes-256-cbc -d -in README.enc.md -out README.dec.md`
Then enter passphrase
`-d` - for decrypt

Compare the files with:
`cmp README.md README.dec.md`
`echo $?` returns 0

#### What is Salt?

Do not want to store passwords as plaintexts.
So we store hashed passwords.

But to prevent bruteforce attacks, we add random string to the plaintext before hashing.
Salt value is a large random string.
So we store salt + password hash in our database.

Just storing hash: same password will have the same hash value
Store both salt+hash: the hash portion will look different even with same passwords

## Asymmetric Key Cryptography

keygen() - produces 2 keys: a public key and a private key
encryt(plaintext, public key) - produce ciphertext
decrypt(ciphertext, private key) - produce plaintext

sign(message, private key) => signature
verify(message, siganture, public key) => returns T/F if signature is valid

-   Without private key, it's hard to produce a message that the public key can verify
-   Hard to forge

Example = rsa

#### Hybrid Encryption

Asymmetric crypto is slow.
In practice we use a combination of asym and sym cryptography.

1. msg
2. sym.keygen() => get sym.key
3. sym.encrypt(msg, sym.key) => ciphertext

Then we send the sym.key with asym.encrypt

1. asym.encrypt(public key, sym.key) => encrypted key

Now the receiver can use their private key to decrypt the sym.key.
Then reconstruct the msg.
