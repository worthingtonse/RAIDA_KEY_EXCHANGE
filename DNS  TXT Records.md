# Domain Name Service Text Record 

Servers need to publish DNS records so that clients can find their RAIDA Key servers used by the server and post the keys to the server. 

If there are no DNS records or DNS names are unknown, then it is assumed that they both have keys(CloudCoins) on the CloudCoin RAIDA (6) and that the place to post the request is on the same host as the one being contacted on port 75. 

## TXT record naming convention
The names of these TXT records always start with the name "raida"
Sample names. 
```
raida.google.com
raida.example.net
raida.myorganization.org
raida.subdomain.example.com
```
## Text inside TXT record

key | Value | Description
---|---|---
host | Fully Qualified Domain Name | use * for all hosts on the domain. % for specified hosts. 
raida | 0 through 65,534 | The RAIDA the server can post keys. 65,534 is for a local RAIDA.
dn | 0 through 255 | The coin denomination the server will use to get the keys
id | 0 through 16,777,216 | The coin Serial Numaber the server will use to get the keys
cp (client_privacy) | 0 -? | Must the client authenticate? 0 for no. 
sa (server_authentication) | 0 -? | Will the server authenticate? 0 for no. 
srv_port | 0-65534 | The location of the server's listener. The default is 75 but this can be any port
srv_target | FQDN | The host name of the server with the listener. 


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
srv_port:75;
srv_target: server.example.com;
"
"
[www2.subdomain.mydomain.com],
raida = 1;
dn = 65200;
id = 983922;
cp = 0;
sa = 1;
srv_port:75;
srv_target: server.example.com;
"
"
[*.mydomain.com],
raida = 42,
dn = 8,
id = 53,
cp = 0,
sa = 1
srv_port:75;
srv_target: server.example.com;
"

"
[%.mydomain.com],
raida = 42,
dn = 8,
id = 53,
cp = 0,
sa = 1
srv_port:75;
srv_target: %.example.com;
"
```
## No Record?
The only records that need to be placed in the TXT are the "rule breakers" and if the server wants to provide mutual authentication. Otherise, the private key exchange services can be used. 

That is any records that do not match the following: 
```
RAIDA = 6
ID = none
ND =  none
cp = 0 (none)
sa = (none)
server port = 75
server target = The same IP address. 
```


<!--
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
-->

