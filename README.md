# RAIDA Key Exchange
An open-source protocol for quantum-safe cryptographic key exchanged. For use as an alternative to SSL/TLS

## 1. Key Storage on clients and servers
Computers must store shared secrets called "Keys". 

### Two Key Types:
  RAIDA Keys (AKA "coins") 
  * Allows the users to send and retrieve encrypted encryption keys to and from the RAIDA.
  * Contains one serial number and twentyfive 128 bit passwords. See format
  * We suggest the use of the open source [Key Manager](https://something.com)  using the standard folder structure.
  
  Encryption Keys: 
  * Encrypts communications between client and server.
  * Contain the name or ID of who the key is for and a 256 bit key.
  * We suggest the use of the open source [Key Manager](https://something.com)   using the standard folder structure. 
	
## 2. Enabling a Server to use RKE
To enable a server to use RKE, a DNS service record and a DNS TXT record explaining the server's RAIDA membership need to be created. Also, an RKE signalling service should be installed on the server. 
- [ ] Add an [RKE signalling service record](https://something.com) to the servers DNS.
- [ ] Add an [RKE Membership TXT record](https://something.com)  to the server's DNS. 
- [ ] Run the [RKE Signalling service](https://something.com)  that listens for RAIDA Requests must be installed using the open-souce RKE signalling protocol. We suggest using the free "raida_key_service" software from RAIDA Tech.   
		
## 3. Enabling a Client to use RKE
For a client to use RKE it must be able to resolve Server RAIDA Membership, Post keys and signal the RKE Enabled Server.
	- [ ] Resolve server signalling service using DNS wrapped in [PLS](https://something.com).
	- [ ] Resolve server membership records using DNS wrapped in [PLS](https://something.com).
	- [ ] Post keys using the [Post Key](https://something.com) protocol. 
	- [ ] Signal the server using the [Signal Server](https://something.com) protocol


## 4. RKE Protocols

Protocol | Description
---|---
[Key Membership Lookup]() | Allows the client to find the RAIDA servers to post keys on that tthe server can access.
[Post Key](https://something.com)  | Posts keys on the RKE RAIDA Servers
Signal Server(https://something.com)  | Allows the client to tell the server about the keys posted
Get Key(https://something.com)  | Allows the server to get keys from the RKE RAIDA Servers. 




4. The RAIDA Name Space
Clients and Servers must be able to share keys through servers that are trusted in common. To facilitate this, a name space of RAIDA has been created so that RAIDA can be located on the Internet. 

Each RAIDA Server has DNS enabled so that it can answer queries about other RAIDA on the Internet. Note the the DNS that is enabled is wrapped within the RKE itself and each request is encrypted using quantum safe encryption. This is a much faster and secure alternative to DOH (DNS over HTTPS).

1. Each RAIDA domain name has a .raida extension. 
2. A typical domain name may look like "cloudcoin.raida", "chat.raida", "bitcoin.raida".
3. Domain names can be registed for $2K with a $1K per year renewal. 
4. Each RAIDA Domain has a number associated with it that computers use behind the seens like IP addresses. 
5. Once registed, owners can configure the IP addresses of all their RAIDA servers. 
6. 
