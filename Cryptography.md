Either system is completely secure or not secure at all. 
Goals of Encryption : 
* Secure communication - Temper traffic, HTTPS
Wireless : WAP2, WEP (Make use of WAP2 as the network encryption protocol and not use WEP.)
* Secure storage Protect data from copy (CSS, AACS). Secure data  -> 
* User Authentication 
* Content Protection

__Symmetric Ciphers__
Encryption : E(k,m) = c           
Decryption : D(k,c) = m 
Requirement D(k, E(k,m)) = m
k=> key m=>message c => cipher    

__Substitution Ciphers__
Ceaser Cipher and Roth 13 ciphers are shifting letters to some n alphabets further. Though key is known for these. Substitution ciphers otherwise would need 26! of keys. Can be broken quite easily.

Large key space doesn’t imply the encryption mechanism is full proof.. 

SSL / TLS(actual layer where encryption takes place) layer 
Handshake : Establish a shared secret Key. available to both parties. 
Record Layer  : Confidentiality and integrity. 



OneTime Pad:  
Shannon proved that one time use of totally random key to XOR data with it secure. But if you can share this long key securely, you should be able to securely send the data itself. 
Its super fast. 
Provides confidentiality but no integrity. People can modify the cipher text without even knowing the key. 

Stream Cipher: Make use of pseudo-Random key to increase the size of the key to XOR the data with. Feed the random key and apply generator to it and then XOR the data with it. 

Misuse: People get lazy and start using same key for encryption. This will very easily start giving you the Xor of messages, and eventually very easy for user to figure out the data..
Example : PPTP: Server and client is used same key to encrypt data for msgs between them. Again resulting in same error. Also called two time key attack. 

Random Generator Get it from hardware interrupts. Intel added support to get it from processor. Thing to note: random generator needs enough seed data to be able to become effective. 

Block cipher 
Fixed size of key (16 bytes). Much slower than stream ciphers.
3DES : 64 bites. slower than AES on latest hardware. 
AES : 128 bites. Configurable Key 128, 192, 256 bits 
Key expansion. 
Run Round functions on the data with key iteratively.  For 10 rounds. Also part of hardware now. 

Pseudo Random Function(PRF)
Defined over (K,X,Y):F:K * X -> Y
Such that exists “efficient” algorithm to evaluate

Pseudo Random Permutation (PRP)
Defined over (K,X,Y):F:K * X -> Y
Such that exists “efficiently” algorithm to evaluate E(k,x), one-one funcation. Possible to decrypt. 

Functionally, any PRP is also a PRF.

CBC: Write notes again

Data Integrity 
Needs a shared key 
CRC : Compute checksum. They are useful to verify data has not been dropped. Or anything like that. But not the fact that data has been corrupted/changed by someone malicious. So checksums without shared key will not work. CRC designed to detect random, not malicious errors

Secure MAC:
Signing Algo : Msg + key -> tag
Verification Algo  : Key + Tag + Msg -> Verify or not verified 

CBC MAC: 
Run CBC on broken data of 16 bytes using key k. Encrypt it one more time with K1. 
Not secure without the last step. 

HMAC: HashMac
SHA - 256: collision resistant. Its very difficult to figure out what this value has been hashed to. But if the size of this mapping goes down to 128 bytes, this becomes easily possible : (
Birthday paradox: you will find a collision in time O(2^(n/)) hashes
How to use it for just integrity: Keep the hashed values at a read only place and let user verify hash of his data matches the hash you got. 
HMAC:  (secure PRF) S(k,m)=H(k XOR opad || H(k XOR  ipad || m))

Parallel Mac:
CBC MAC and HMAC are made sequential . break data into 16 blocks. Apply a function P to Key and index of msg. XOR data to it. And finally XOR data to all these outputs. 

Timing attack: based on timing of when the verification fails use is able to determine he has been able to get which section of data right. 



Authenticated Encryption
Confidentiality on its own is never used. 




Public key encryption: 
Something encrypted using public key, can only be decrypted using the private key. 
∀(publicKey,privateKey) : ∀ m∈M:Decrypt(privateKey, Encrypt(publicKey,m))=m

Trapdoor function: one way function. You can’t compute message without knowing private key. 

How to use RSA? 
Its just used to choose which key to use for encryption of the messages itself. 

How file sharing works in public system?
The person will select a Key using which the file will be encrypted. Then in the file header, the selected key is kept in encrypted form. This file is encrypted using my public key. When I want to share it with Someone else, i would also encrypt it with their public key and put it in header. 

Digital signature Bind the document to author of document. 
Trapdoor permutation same as trapdoor function. The only difference being the function F will map the data from domain X -> X. but its still a one way function. 
publicK, privateK


Opposite of Public Key Encryption: User would apply the one way the data’s hash with his private key. And then anyone can apply the function with Public key to decrypt it. 
sign(privatek,m):=F‐1(priavtek,H(m))
Verify(publick,m,sig):=accept if F(pk, sig) == Hash(m) rejectotherwise
Hash needs to be collision resistant hash function. 

MAC can also do same thing, but MAC needs a new key for everyUser. This is good when we have more people who would want to verify the signature. 

Certificates:
How do I know that I am talking with correct server? Cert Authorities bind public keys to a server.
Both can check with a cert authority and using that gives us a go ahead that we can talk with one another.. Or the client can just talk with server and verify the sert. 
Key Exchange 
TLS : 

Doesnt have forward secrecy. If you want that somehow  you need to be able to pass in another param which is used to generate PreK and is deleted right after. 

Deffie Helman : 

Makes use of nonces 
Makes use of a temp set of Public-Private key set, which will be used to generate PreK and then temp a and b are deleted. 
The data that is sent across is signed by private key of server to Prevent Man in Middle attack. 

Non-interactive Key share : (Read more about it online)

Authenticating users 
Server wants to authenticate whom its talking with. 
But we also have use cases where none of the party know with whom they are talking.. 
Types of attackers : 
Direct Attacks : Attack the server where you feel the password might be present. .
Storing password on server:  Hash the password using a Oneway hash(we cant figure out the password from the hash). and salt it-> effective attack against online dictionary attack.
Online Dictionary attack : Try a huge set of passwords on the accounts because ¼ of the users will have passwords which will fall into this dictionary. Basic way to stop it : delay the response to the request in case of failure by twice.. And put hard limit on number of incorrect attempts. Server can stop logins if lots of passwords are tried from the same password. It’s possible to break a password which <= 7 letters in few mins. Using a slow hash function can also slow down the attacker by huge amt. 
Use bio-metrics: Issue in case its compromised, no way to change it. 


Eavesdropper attacker : can be prevented using one time password system. Password + one time gen key. Uses AES-128. Problem, server needs to keep the secret keys with it. 
Way to, hash the password some k (huge number) of times and then server is sent it password which has been hashed k-1 times and server verifies with it.. 

Active attackers: Offline Fake attack. Also send random msgs Challenge : response would need a keyboard and display.. 2 things basically to make sure there is no fake person in between.. 


THINGS TO READ MORE
Anonymous communication(Tor): makes uses of random proxy, with each hop having some hashing being performed on it. 

Digital cash User doesn’t want to reveal who I am to the merchant. (NEED TO LEARN MORE ABOUT IT)

Multiparty computation
Fully homomorphic computation: 
Zero knowledge proof
Group Signature share a group signature 
Broadcast Encryption:  












Application security 
Authenticate -> verify who you are. 
Authorization-> Ensure User is able to only access the files/data accessible to this user. 



Foundation Of Security book. 



What’s the use of 8080 ? 
Whats port 80 use ? 

