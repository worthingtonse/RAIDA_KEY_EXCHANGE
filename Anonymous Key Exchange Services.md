# Anonymous Key Exchange Services
This allows the client and the server to exchange keys without any mutual authentication. 

Command Code | Service 
--- | --- 
41 | [Post Key](DA%20Key%20Tickets.md#post-key) 
42 | [Get Key](RAIDA%20Key%20Services.md#get-key) 
43 | [Signal](RAIDA%20Key%20Services.md#signal) 

# POST KEY
The computer that wants to initiate a private session between uses this service to post keys for the other computer to download. This service uses UDP. 

1. The client will start by generating a key that is at least 128 bits (16 bytes).
2. The last byte of this key will be 0xFF. 
3. They client will then devide the key into difference sizes that will be 4 bytes, 8 bytes, 16 bytes, 24 bytes 32 bytes or 40 bytes. 

The key parts are then posted to different RKE servers using this protocol: 

 Code | Meaning
 ---|---
 CH | Challenge
 ID | Key ID
 KS | Key Start byte index within the padding field 0-127
 KL | Length of key.  Must be 4, 8, 16, 24, 32 or 40. Other numbers are invalid
 00 | Padding. Random numbers to help keep the key encrypted secretly. 
 HS | Hash of Key ID 
 
Sample request showing an 8 byte key hidden in the padding starting at the 19th byte.  164 Btyes fixed. 
```hex
CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH  //Challenge
ID ID ID ID ID ID ID ID ID ID ID ID ID ID ID ID //Key ID
KS 
KL
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 KY KY KY KY KY KY KY KY 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
E3 E3  //Not Encrypted 
```
NOTE: The Key Start and the Key Length help to strengthen the encryption. It also reduces DOS attacks designed to use up system resource.

The key used to encrypt the request will write over any other requests it has made. Each encryption coin can only make one request at a time and is the Primary key in the RKE server's key table. 


Response Status Codes
Code | Status | Details | Implemented Now?
---|---|---|---
0 | Fail | failed but no details provided | Yes
1| Success | Key posted successfuly | Yes
3 | Fail: Client's IP Address was zero. Anonymous clients not allowed. | No
4 | Fail: Receivers's IP Address was zeros. Anonymous receiver not allowed.| No
5 | Key Start was invalid. |Yes
6 | Key Length was invalid. | Yes
7 | Fail: Coin used for encryption was used too many times. | No
8 | Offline | Service temporarilty offline  | No

Sample Response Body
```
E3 E3 //Not Encrypted
```



# GET KEY
Allows a client to get a key on an RKE server that was left for them by a computer initiating a private conversation.  

Sample Request fixed size: 
```hex
CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH 
HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS //Hash of Key ID 
E3 E3  //Not Encrypted
```

Response Status Codes
Code | Status | Details
---|---|---
0 | Fail | failed but no details provided
1| Success | Key downloaded successfuly
2| Fail: Client Authentication | Authentication was required but not provided
3| Fail: Client Authentication, Bad User| the receiver account does not exist
4| Fail: Client Authentication, password was incorrect.
5| Fail: Anonymouse Receivers not permitted | No reciever address was provided and that is not allowed
6| Fail: Key Not found | the SN of the sender does not exist
7| Fail: AN and PN were the same. | The PN must change everytime
8 | blocked Sender| Client was blocked because of client's SN
9 | blocked Receier | Sending keys to that receiver is not permitted. 
10 | blocked PAN | Your AN was the same as the PAN. They must be different on this server. 
10 | Offline | Service temporarilty offline 
12 | Fail: Key not found | There was not key with that ID on the RAIDA. 



Sample Respons fixed size: 
```hex
ID ID ID ID ID ID ID ID ID ID ID ID ID ID ID ID //Key ID Hash
DN  SN SN SN SN //The encryption ID of the key sender 
IP IP IP IP IP IP IP IP IP IP IP IP I4 I4 I4 I4 
E3 E3  //Not Encrypted
```


# SIGNAL

Tells the Receiver that there are keys waiting for it. 

Code | Meaning
---|---
CH | Challenge to server
NU | Number of key parts that need to be retrieved
HS | Hash of the key ID
IP | Version of IP. Either 4 or 6
IP | IPv4 of key server or first 4 of IPv6
00 | Last 12 of IPv6 of key server or random numbers if IPv6
PT | Port used by the key server


Sample Request with 4 key parts(Unencrypted):
```hex
CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH  
NU
IP
HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS 
IP IP IP IP 00 00 00 00 00 00 00 00 00 00 00 00  PT PT PT 
IP
HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS 
IP IP IP IP 00 00 00 00 00 00 00 00 00 00 00 00  PT PT PT
IP
HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS 
IP IP IP IP 00 00 00 00 00 00 00 00 00 00 00 00  PT PT PT
IP
HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS HS 
IP IP IP IP 00 00 00 00 00 00 00 00 00 00 00 00  PT PT PT 
E3 E3 //Not Encrypted
```

The response will have a fixed 18 bytes. This response will be
Sample Response to Sender:
```hex
FF FF 00 FF 00 00 00 00 00 FF 00 00 00 00 00 00 //Byte fields showing which key parts were used. 
E3 E3  //Not Encrypted
```
Sample Response Body SHowing that key parts 0, 1 and 3 where used and key part 


<!--
# EXCHANGE KEYS 
Command #220 uses TCP fixed 1016 bytes. 
This is used by a client to tell a server where to go get keys. This uses TCP Protocol. The client sends the server a list
of IP addresses where keys can be found. The server then attempts to collect the keys. 
If it gets all the keys it says status "good". If it gets no keys then it says satus "bad. 
If it gets some of the keys it says status "mixed". It will then return which keys it was able to ret in a bitfield. 
The client will need to change its key depending on the response of the server. 

There are 50 slots for IP addresses. 

The request body has a total fixed number of bytes of 1016 bytes excluding the ending bytes.

Bytes in the Exchange Key Request Body
Code | Name | Total Bytes
---|---|---
CH | Challenge | 16
VR  | IP version: 4 = IPv4, 6 = IPv6 or 0 = "empty slot" | 25
CI | Cacheable Key ID | Maybe different on different key servers | 16  
ID | Exchange Request ID generated by the client so that keys can be cached on the Key Serer | 400
CO | Cloud or Coin ID
SP | Split Number
DA | Data Agent (RAIDA ID)
SH | Shard ID
DN | Denomination of the sender's ID on the RKE server | 25 
SN | Four byte serial number of the user on the RKE Server | 100
TOTAL | fixed number of bytes not including the EA |1016

Sample Request: 
```hex
CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH 
CI CI CI CI ID ID ID ID ID ID ID ID ID ID ID I D//Cacheable Key ID that can be cached retreived. 
CO CO SP DA SH DN SN SN SN SN //Place and ID that the server should log in. 
DN SN SN SN SN ID ID ID ID ID ID ID ID ID ID ID //Key ID
CO CO SP DA SH DN SN SN SN SN //Place and ID that the server should log in. 
DN SN SN SN SN ID ID ID ID ID ID ID ID ID ID ID //Key ID
... There are 50 retrievable key slots. Version zero is means empty slot and that is the last one. 
CO CO SP DA SH DN SN SN SN SN //Place and ID that the server should log in. 
DN SN SN SN SN ID ID ID ID ID ID ID ID ID ID ID //Key ID
E3 E3 //Not Encrypted
```

Response Status Codes for Exchange Keys
Code | Status | Details
---|---|---
0 | All Fail | failed to get any keys - warning to shut off 
1| All Success | Downloaded all the keys successfully 
2| Mixed Success | Some of the keys were received 
3| Found Cached | The cached key was found and is being used. 
10 | Offline | Service temporarilty offline 

Sample DNS TXT Record:
```
TXT 
name=raida 
Content=
"r0=RKE.secure.com 89828293 30000 " "r1=RKE.secure.com 89828293 30000 " "r2=RKE.secure.com 89828293 30000 " "r3=RKE.secure.com 89828293 30000 " "r4=RKE.secure.com 89828293 30000 " "r5=RKE.secure.com 89828293 30000 ""r6=RKE.secure.com 89828293 30000 " "r7=RKE.secure.com 89828293 30000 " "r8=RKE.secure.com 89828293 30000 " "r9=RKE.secure.com 89828293 30000 " "r10=RKE.secure.com 89828293 30000 " "r11=RKE.secure.com 89828293 30000 "
```
Sample allowing anonymous:
```
TXT 
name=raida 
Content=
anonymous=ok
```

Sample Response:
```
MS MS ... //there will only be a response body if the status is mixed.
EA EA EA  //Not Encrypted
```

# Free Account
Command #230 fixed bytes over UDP
This allows people to start an account on a RKE server the RKE server they are free

Sample Request: 
```hex
CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH 
DN  SN SN SN SN  //Proposed key ID
CH CH CH CH 
EA EA EA  //Not Encrypted
```

Sample Response 
Status Good 
```hex
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
```

# Paid Account
Comand #240 fixed bytes over UDP. This must be called over SSL unless there is already some common keys. 

Allows people to create an account on a RKE server and get AES keys. 

Sample Request: 
```hex
CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH CH 
R D  SN SN SN  AN AN AN AN AN AN //Authenticates
ND  SN SN SN SN 
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
....... 25 Authenticty numbers 16 bytes each. 
CH CH CH CH 
EA EA EA  //Not Encrypted
```

Response Status Codes
Code | Status | Details
---|---|---
0 | Fail | failed but no details provided
1| Success | AN attached
2| Fail: ID already in use |
3| Fail: ID outside of range |
4| Fail: CloudCoin was counterfeit |
5| Fail: CloudCoin was not valuable enough. |
6| Fail: No Paid Service. Use Free instead|
7| Fail: Too many account requests try again later.|
10 | Offline | Service temporarilty offline 


Sample Response 
Status Good 
```hex
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
```
-->
