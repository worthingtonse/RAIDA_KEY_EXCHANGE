# RAIDA Key Exchange
An open-source protocol for quantum-safe cryptographic key exchanged. This enable the use of the [Presentation Layer Security protocol](https://som.com) as an alternative to TLS. 

## 1. Key Storage on clients and servers
Computers must store shared secrets called "Keys". 

### Two Key Types:
  RAIDA Keys (AKA "coins") 
  * Allows the users to send and retrieve encrypted encryption keys to and from the RAIDA.
  * Contains one serial number and twentyfive 128 bit passwords. See [Standard format](https://www.com)
  * We suggest the use of the open source [Key Manager](https://something.com)  using the standard folder structure.
  
  Encryption Keys: 
  * Encrypts communications between client and server.
  * Contain the name or ID of who the key is for and a 256 bit key.See [Standard format](https://www.com)
  * We suggest the use of the open source [Key Manager](https://something.com)   using the standard folder structure. 
	
## 2. Enabling a Server to use RKE
To enable a server to use RKE, a DNS service record and a DNS TXT record explaining the server's RAIDA membership need to be created. Also, an RKE signalling service should be installed on the server. 
- [ ] Add an [RKE signalling service record](https://something.com) to the servers DNS.
- [ ] Add an [RKE Membership TXT record](https://something.com)  to the server's DNS. 
- [ ] Receive Key Signals using the [Signal Service](https://something.com) protocol
- [ ] Get keys using the [Get Key](https://something.com) protocol. 
		
## 3. Enabling a Client to use RKE
For a client to use RKE it must be able to resolve Server RAIDA Membership, Post keys and signal the RKE Enabled Server.
- [ ] Resolve server signalling service using DNS wrapped in [PLS](https://something.com).
- [ ] Resolve server membership records using DNS wrapped in [PLS](https://something.com).
- [ ] Post keys using the [Post Key](https://something.com) protocol. 
- [ ] Signal the server using the [Signal Service](https://something.com) protocol


