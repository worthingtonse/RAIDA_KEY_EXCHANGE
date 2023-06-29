# RAIDA Key Exchange

![RAIDA Key Exchange](https://github.com/worthingtonse/RAIDAX/blob/CBDC/Server/rke.jpg)
An open-source protocol for quantum-safe cryptographic key exchanged. This enable the use of the [Presentation Layer Security protocol](https://som.com) as an alternative to TLS. 

## 1. Key Storage on clients and servers
Computers must store shared secrets called "Keys". 

### Two Key Types:
  RAIDA Keys (AKA "coins") 
  * Allows the users to send and retrieve encrypted encryption keys to and from the RAIDA.
  * Contains one serial number and twentyfive 128 bit passwords. See [Standard format](https://github.com/worthingtonse/RAIDA_KEY_EXCHANGE/blob/main/Key%20Coin%20Format.md)
  * We suggest the use of the open source [Key Manager](https://something.com)  using the standard folder structure.
  
  Encryption Keys: 
  * Encrypts communications between client and server.
  * Contain the name or ID of who the key is for and a 256 bit key.See [Standard format](https://github.com/worthingtonse/RAIDA_KEY_EXCHANGE/blob/main/Encryption%20Key%20Format.md)
  * We suggest the use of the open source [Key Manager](https://something.com)   using the standard folder structure. 
	
## 2. Enabling a Server to use RKE
To enable a server to use RKE, a DNS service record and a DNS TXT record explaining the server's RAIDA membership need to be created. Also, an RKE signalling service should be installed on the server. 
- [ ] Add an [RKE Membership TXT record](https://github.com/worthingtonse/RAIDA_KEY_EXCHANGE/blob/main/DNS%20SRV%20and%20TXT%20Records.md)  to the server's DNS. 
- [ ] Receive Key Signals using the [Post RAIDA Key](https://github.com/worthingtonse/RAIDA_KEY_EXCHANGE/blob/main/Key%20Exchange%20Services.md#post-raida-key) protocol

		
## 3. Enabling a Client to use RKE
For a client to use RKE it must be able to resolve Server RAIDA Membership, Post keys and signal the RKE Enabled Server.
- [ ] Resolve server membership records using DNS wrapped in [PLS](https://something.com).
- [ ] Encrypt Keys using the [Encrypt Key](https://github.com/worthingtonse/RAIDA_KEY_EXCHANGE/blob/main/Key%20Exchange%20Services.md#encrypt-raida-key) protocol. 
- [ ] Post Encryption Key the server using the [Signal Service](https://github.com/worthingtonse/RAIDA_KEY_EXCHANGE/blob/main/Key%20Exchange%20Services.md#post-raida-key) protocol


