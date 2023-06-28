# Domain Name System Service and TXT Record Standard 

Servers need to publish DNS recoreds so that clients can find their RAIDA Key servers and the signal point to tell the server that keys have been sent. 

Before a person can receive keys from another user, they must have a DNS record that points to the RKE servers that the receiver has trust/coins with.
However, if there are not DNS records or DNS names are unknown, then it is assumed that they both have keys(CloudCoins) on the CloudCoin RAIDA. 

Sample DNS TXT Record:
There are three types of DNS records that can be used:
1. Entire RAIDAs. These are the Coin IDs that are part of the RAIDA Namespace (0-65535). They specify entire RAIDA clouds that have RKE enabled. For exmple, the Coin ID 1 would mean that all 25 CloudCoin RAIDA can be used.
2. Individual RKE servers. An array of specific RKE servers. These RKE may not be parts of other RAIDA or parts of coins. 
3. Anonymous. This allows the client to specify the RAIDA and the server will then join these RAIDA


## TXT record naming convention (raida.domain.tld)
The names of these txt records always start with the name "raida"
Sample names. 
```
raida.google.com
raida.example.net
raida.myorganization.org
raida.subdomain.example.com
```

key | Value | Description
---|---|---
host | Fully Qualified Domain Name | use * for all hosts on the domain.
raida | 0 through 65,534 | The RAIDA the server can post keys
dn | 0 through 255 | The coin denomination the server will use to get the keys
id | 0 through 16,777,216 | The coin Serial Numaber the server will use to get the keys
client_privacy | yes or no | Must the client leave their key ID?
server_authentication | yes or no | Must the server leave their key ID?



### Sample Coin ID DNS TXT record named "RKE"'s content:
```ini
"[notes],
To parse = Delete all quotes and replace commas with line breaks. Then treat as .ini file;

"
"
[www.mydomain.com];
raida = 1;
dn = 16;
id = 983922;
cp = 0;
sa = 1;
srv_port:9998
srv_proto:TCP;
srv_target: server.example.com;
"
"
[www2.subdomain.mydomain.com],
raida = 1;
dn = 65200;
id = 983922;
cp = 0;
sa = 1;
srv_port:9998;
srv_proto:TCP;
srv_target: server.example.com;
"
"
[*.mydomain.com],
raida = 42,
dn = 8,
id = 53,
cp = 0,
sa = 1
srv_port:9998;
srv_proto:TCP;
srv_target: server.example.com;
"

"
[%.mydomain.com],
raida = 42,
dn = 8,
id = 53,
cp = 0,
sa = 1
srv_port:9998;
srv_proto:UDP;
srv_target: %.example.com;
"
```
In the example above, the key receivers is advertising that it has shared secrets (coins) with the RAIDA IDs of 1, 2, 17 and 65023.  The client will then need to ask the Guardians for host records for the locations of these RAIDA. And the client may need to get coins to talk to these RAIDA. 

### Sample Indvidual DNS TXT record named "RKE"'s content:
```
"r0=RKE.secure.com 89828293 30000 " "r1=RKE.secure.com 89828293 30000 " "r2=RKE.secure.com 89828293 30000 " "r3=RKE.secure.com 89828293 30000 " "r4=RKE.secure.com 89828293 30000 " "r5=RKE.secure.com 89828293 30000 ""r6=RKE.secure.com 89828293 30000 " "r7=RKE.secure.com 89828293 30000 " "r8=RKE.secure.com 89828293 30000 " "r9=RKE.secure.com 89828293 30000 " "r10=RKE.secure.com 89828293 30000 " "r11=RKE.secure.com 89828293 30000 "
```
Here the receiver supports 12 RKE servers and they all encrypt with key number 89,828,293 and use port 30,000.

Sample Anonymous DNS TXT record named "RKE"'s content:
```
"anonymous=ok"
```
 ### It is also possible to mix them:
 ```
 "1" "r0=RKE.secure.com 89828293 30000 " "anonymous=ok"
 ```

## Client Receiver Resolution
1. The client will lookup the receivers RKE TXT record using a call to DNS. 
2. The client then parse the TXT file.
3. The client will decide on a strategy of which RKE servers to use.
4. The client resolves the IP addresses of the RKE servers (if needed). 

## Ticket Getting. 
In the ticket getting phase, the client will create a key and then have the receiver's RKE servers encrypt it. 

