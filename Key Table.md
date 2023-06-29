# Key Table

This is the table that is stored on the RKE Server

Each Record is 70 bytes. 
 
Column | Datatype | Note | Required?
 ---|---|---|---
Sender's Encryption Coin | 5 bytes (DN SN SN SN) |Primay Key. This is coin used by the client to encrypt their key part to the RKE server.| Yes
Receiver's Encryption Coin | 5 bytes (DN SN SN SN) |Primay Key. This is coin used by the client to encrypt their key part to the RKE server.| No
Sender's IP Address | 16 bytes | IPv6 uses all 16 bytes. IPv4 uses just the last 4 with all other values being zero. Can be zeros | No
Receiver's IP Address | 16 bytes | The computer that is allowed to get the key parts. Can be zeros | N0
Key ID MD5 Hash | 16 Bytes | GUID | Yes
Key Length | 1 byte | should be 1 through 16. |Yes 
Key Part | 16 Bytes | Fixed but only the last bytes that match the key lenght are used. | Yes
