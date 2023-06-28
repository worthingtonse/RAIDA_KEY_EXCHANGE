# RAIDA Key Exchange Services

This is used to exchange keys between clients who have shared secrets with RAIDA servers but not a specific RAIDA server. 
This will establish a shared secret with a fracked RAIDA so fixes can be done. 

Command Code | Service | Done in Rust | Done in C | Done in C2
--- | --- | :---: | --- | ---
44 | [Encrypt RAIDA Key](RAIDA%20Key%20Tickets.md#post-raida-key) |  | 🟢|
45 | [Post RAIDA Key](RAIDA%20Key%20Services.md#post-raida-key) |  |🟢 |



## ENCRYPT RAIDA KEY

Note: In Phase I, Clients know that the RAIDA will each have 1000 coins on their 0 network and the client can pick one at random to encrypt the target RAIDA's 
Sample Request:
```hex
CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH  //Challenge
DN SN SN SN SN //The ID of who the key is for. // The Denomination, The Serial Number of the Bob's encryption coin (1-25,999) depending on Bob's RAIDA ID. 
KY KY KY KY KY KY KY KY //Eight byte key part. 
DN SN SN SN SN //The serial number of the coin that will be used to create a shared secret between Alice and Bob (Fracked coin on Bob)
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN //The AN of the coin used to create a shared secret between Alica and Bob (Fracked coin on Bob)
E3 E3  //Not Encrypted

Example: 
EB6BD09D809743DFA8955BBF96274CC5
64C02B25EE
BE4CDFAE29887C11
B25EE64C02
9887C11BE4CDFAE2
E3E3
```

The RKE will take 8 byte key parts and then add 5 bytes of the coin to be fixed (DN SN SN SN SN), 2 random bytes to the end. 
Then it adds one know byte to the end: 0xFF. 
This creates a 16 byte block that can be encrypted effectivly and padds it as needed. 
Then, the RKE server will use the encryption key that the client specified to encrypt the 16 bytes. 
The RKE returns the encrypted 16 bytes to the user.

The nonce for encryption is the same as the nonce sent in the request header.

Response Status Codes
Code | Status | Details
---|---|---
0 | Fail | Failed but no details provided
1| Success | Key ticket returned successfuly
11 | Offline | Service temporarilty offline 

Sample Response Body
```
D798010F14C64102AC3EB6D728166054
E3 E3 //Not Encrypted
```

## POST RAIDA KEY
Phase I: This service only runs on the RAIDA. It allows clients to tell the RAIDA to decrypt key parts so the communications can be encrypted. 

How it works in Phase I: 

The "Post Key" service recieves a group of tickets encrypted by other RAIDAs. It uses its Authenticity Numbers of its own keys to decrypt them. Then it assembles the keys that it is able to encrypt a response to the caller. It checks that each key ends with a "0xFF"and removes the last eight bytes that the client does not know about. It Ignores keys it is unable to encrypt and then respond to the caller with:

* Which key parts it used to create a master key.

* A SHA-256 hash of the key.

* Seems the Nonce in the header should be the same as the ones used in the "Encrypt Key" requests.

Sample Request. Note: This is all enencrypted:
```hex
CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH  //Challenge
DN SN SN SN SN //The serial number of the fracked coin. 
CO CO SP DA SH DN SN SN SN SN KY KY KY KY KY KY KY KY KY KY KY KY KY KY KY KY //Coin to encrypt and key part. 
CO CO SP DA SH DN SN SN SN SN KY KY KY KY KY KY KY KY KY KY KY KY KY KY KY KY //Coin to encrypt and key part.  
.....
CO CO SP DA SH DN SN SN SN SN KY KY KY KY KY KY KY KY KY KY KY KY KY KY KY KY //Coin to encrypt and key part. 
E3 E3  //Not Encrypted
```

Meaning of Codes:
Code | Meaning
---|---
CH | Challenge
CO | Coin ID
SP | Split ID (Zero if no splits)
DA | Detection Agent (RAIDA ID)
SH | Shard ID
DN | Denomination 
SN | Serial Number
KY | Key
E3 | Ending bytes


The service will then decypt the keys using the keys that it owns. 
It looks for the 0xFF at the end to make sure the coin is whole and true. 
It removes the coin DN and SN SN SN SN bytes and make sure they are the same for both key parts. 

Then it responds to the client with a hash of the key it has created. 

RAIDA Changes the AN in its ANs table for the coin specified in the key parts. 

Sample Respons:
```hex
HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS //SHA-256 hash of the Master key or is it SHA-128?
KA //Key accepted status. Either 0x00 for not accepted or 0x01 for accepted. 
KA
---
KA //one for every key the client proposed
E3 E3  //Not Encrypted
```

When the client sends the key (Post Key), it can put the key parts in any random order. The RAIDA should keep the key parts in the same order. 


Client "Alice" wants to talk to RAIDA 11 "Bob". However, Bob lost all its data and Alice and Bob have no shared secret.  RAIDA 5 is "Carol" and RAIDA 12 is "Dan". Alice has shared secrets with Carol and Dan. 

Step | Actor | Action | Notes
---|---|---|---
1 | Alice | Gets the AN from a coin that is fracked on Bob. This is the AN that Bob should have. | This is a GUID
2 | Alice | Chooses 2 random RAIDA to talk to | In this case Carol and Dan are chosen
3 | Alice | Alice breaks the key into 2 parts (Each 8 bytes long) | Parts are indexed as part 0 and part 1
4 | Alice | Encrypts the request including the DN, SN and AN of the coin on bob to be fixed along with part 0 usings Carol's key  | This is the AN for the coin that we want to fix on Bob.
5 | Alice | Encrypts part 1 using Dan's key | Again, We will use the encrypt DN SN SN SN SN of the coin we will fix on Bob.
6 | Alice | Calls the [Encrypt Key](RAIDA%20Key%20Tickets.md#post-raida-key) service on Carol and Dan | Tells them to encrypt using Bob's key
7 | Carol & Dan | They unencrypt the request using Alice's shared secret | This is the coin in the request header. We want to fix this on Bob.  
8 | Carol & Dan | Add padding and encrypt the part using Bob's key | Five bytes are from the coin in the request header's encryption part. 
9 | Carol & Dan | Encrypt the response using Alice's key and return to Alice | Now Alice has unreadable "tickets"
10 | Alice | Does not encrypt the body of the [Post Key](RAIDA%20Key%20Services.md#post-raida-key) request to Bob | Denomination and Serial Numbers are clear text
11 | Bob | Bob decrypts the messages using Carol's and Dan's Key | Must already have shared secrets with other RAIDA
12 | Bob | Puts the key parts together in the order the were received | First keypart is part 0. Second is part 1
13 | Bob | Bob puts this key into the AN of the coin specified | Now Alice and Bob have at least one shared secret
14 | Bob | Bob uses the key to encrypt the response | Responds to Alice
15 | Alice | Alice decrypts the response and checks the challenge | 
16 | Alice | If the challenge is good, Alice changes the AN on the coin she specifies to the key  | Not neccessar if the AN has not changed. Now Alice has a common secret with Bob
17 | Alice | Alice can now make any request she likes to Bob using this coin | Key exchange done
