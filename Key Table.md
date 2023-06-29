# Key Table

This is the table that is stored on the RKE Server.

This table is only used when the client and the server will connect without authenticating each other. This is used for encryption between unknowns. 

Each Record is 33 bytes. 

Keys can only be retrieved once. 

Column | Datatype | Note | Required?
 ---|---|---|---
Hash of Key ID | 16 Bytes | MD5 hash of GUID | Yes
Key Length | 1 byte | should be 1 through 16. |Yes 
Key Part | 16 Bytes | Fixed*  | Yes

*If the key is less than 16 byte, the first bytes are the key and extra bytes are zeros. 
