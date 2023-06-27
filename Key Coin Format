# RAIDA Key File Format
This is the format that clients have that allow them to encrypt communictions between themselves and the RKE RAIDA. These are also coins. 

# Binary Files
Binary files have a ".bin" extention. They may hold many coins. PNG files are .bin files stored inside of PNGs. 
Each file has one file header. Each coin inside the file has one coin header. So you will always have a file header and one coin header for each coin. 

See the PNG file here: 
https://hierodev.com/notes/

The primary format for keys both inside and outside of software will be Format 10 (0x0A).

## File Nameing Convention For Single Coins Inside Software:
We are accurate to one satoshi which represents a decimal, seven zeros, and a 1.

Denomination Whole|   Satoshies (Fraction) | BTC|  Pown String | SN | extra separator | Price | Group Name | Extention
---|---|---|---|---|---|---|---|---
0|  00_000_001| BTC |  ppppfpppuppppppppppupppep| 20 | extra | 10 | MyGroup | .bin
0|  00_000_010| BTC |  ppppfpppuppppppppppupppep| 87920 | extra | 10 | MyGroup | .bin
0| 01_000_000| BTC |  ppppfpppuppppppppppupppep| 499920 | extra | 10 | MyGroup| .bin
1| 00_000_000| BTC |  ppppfpppuppppppppppupppep| 73920 | extra | 20 | Your Group | .bin
100| 00_000_000| BTC |  ppppfpppuppppppppppupppep| 2332920 | extra | 1000 | MyGroup New | .bin

Samples:
```dos
0.00_000_001.BTC.ppppfpppuppppppppppupppep.488773920.extra.20..bin
0.00_100_000.BTC.ppppupppeppfpppuppppppppp.1122320.extra.30.Group.bin
0.00_000_001.BTC.ppppupppeppfpppuppppppppp.223920.extra.40..bin
1.00_000_000.BTC.ppppupppeppfpppuppppppppp.94887720.extra.50..bin
1000.00_000_000.BTC.ppppupppeppfpppuppppppppp.3920.extra.1000.All people.bin

```

## File Nameing Convention For Files with Multiple Coins:
If there are more than 1 coin in the file, the sum of all the coins will be in the name. The extension maybe .bin or .png. 

Denomination Whole|  BTC |  Satoshies (Fraction) | SAT|  Pown String | SN | Extention
---|---|---|---|---|---|---
0| BTC | 00_000_001| SAT |  tag| 20 | .bin
0| BTC | 00_000_010| SAT |  memo| 87920 | .png
0| BTC | 01_000_000| SAT |  note| 499920 | .bin
1| BTC | 00_000_000| SAT |  custom text| 73920 | .bin
100| BTC | 00_000_000| SAT | empty| 2332920 | .bin


Samples:
```dos
0 BTC 08_362_541 SAT.For Billy.bin
0 BTC 50_167_000 SAT.bin
0 BTC 00_400_099 SAT.Taylor.bin
1 BTC 00_000_340 SAT.transport.bin
1268 BTC 69_894_485 SAT.CUBBY.bin
1268 USD 69_894_485 CENTS.CUBBY.bin

```


## File Header For All Coins (32 Bytes Fixed)
```
FT CL ID ID SP EN CC HS HS HS HS HS HS HS HS FL
RC RC RC RC RC RC RC RC RC RC RC RC RC RC RC RC 
```
## File Header Codes:
Code | Bytes |  Name | Description
---|---|---|---
FT | 1 |  Format Type | '9', 'A' or 'B'.
CL |  1 | Cloud ID | 1 for theRAIDAX network
ID |  2 |Coin ID 1 | ID Coins are 0, CloudCoin is 1, NFTs are 2  10 =  BTC
RE |  1 |Reserved| Reserved
EN |  1 |Encryption Type | 0 for no encryption
CC |  2 |Coin Count | How many note are in the file. Not the tota value but total count. 
HS |  7 | MD5 Hash of encryption password | First 7 bytes of a hash of the encryption key used to encrypt the coin seeds. (if used). Otherwise zeros if no encyption
FL |  1 |Flags | Represents 8 bitfields. 00000000 means no flags. 00000001 means encoded with email recovery. Custom for applications. 
RC |  16 | Receipt Number | A transaction's GUID to help software track transactions internally. Can be all random or zeros. But can be used by applications for anything. 

## DENOMINATIONS

Except for IdCoin and possible future extensions of this table decimal values should be treated as powers of ten

Hex Code| Decimal Code | Denomination
--- | :---: | ---
`0xf8` | F8 | `.000 000 01` (1 Satoshi)
`0xf9` | F9 | `.000 000 1`
`0xfa` | FA | `.000 001`
`0xfb` | FB | `.000 01`
`0xfc` | FC | `.000 1`
`0xfd` | FD | `.001`
`0xfe` | FE | `.01` (1 Cent)
`0xff` | FF | `.1` (1 Dime)
`0x00` | 00 | `1` (1 whole)
`0x01` | 01 | `10`
`0x02` | 02 | `100`
`0x03` | 03 | `1,000`
`0x04` | 04 | `10,000`
`0x05` | 05 | `100,000`
`0x06` | 06 | `1,000,000`
--- | --- | ---
`0x80` | 80 | IdCoin
--- | --- | ---
|  x | XX  | Others (Application keys, chat, etc...) 



# FORMAT '8'
This requires 807 bytes per coin. Format 8 should be readable by all software but is not the perfered file format. Format 'A' should be used instead. 
This format is for internal use only. It is used for coins that are stored in the Limbo folder

Shard 1 byte | Stripe 1 byte | Denomination 1 Byte | Serial Number (4 Bytes) | Authenticity Numbers | PAN Numbers
---|---|---|---|---|---
SP | ST | DN | SN SN SN SN  |  16 byte AN x 25 | 16 byte PAN x 25


## Coin Body Codes for Format 9
16 byte AN x 25 (The original ANs before the call)
16 byte PAN x 25 (The PANs that were sent to the RAIDA but were not confirmed)

Sample coin in format 8
```hex
SP ST DN SN SN SN SN 
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN   
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN   
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  

```



# FORMAT '9'
This requires 407 bytes per coin. Format 9 should be readable by all software but is not the perfered file format. Format 'A' should be used instead. 

Shard 1 byte | Stripe 1 byte | Denomination 1 Byte | Serial Number (4 Bytes) | Authenticity Numbers
---|---|---|---|---
Shard | ST | DN | SN SN SN SN  |  16 byte AN x 25


## Coin Body Codes for Format 9
16 byte AN x 25

Sample coin in format 9
```hex
SP ST DN SN SN SN SN 
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN   
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  
AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN AN  

```




# FORMAT 'A'
## Coin Header Codes for Format 'A'
Every coin within a file has its own header. For file format 'A', we just have 20 bytes per coin because all the ANs are created by one hash seed. 

Shard | Denomination 1 Byte | Serial Number (4 Bytes) 
---|---|---
SD| ST | DN | SN SN SN SN  

## Coin Body Codes for Format 'A'

 11 Byte Seed (SE ED) | 3 Bytes Reserved (RE)
---|---
 SE ED SE ED SE ED SE ED SE ED SE | RE RE RE


##  Sample Coin with Header and Body Format 'A'
This has 7 coins in it. Each coin takes up 20 bytes. 
```
SD ST DN  SN SN SN SN  SE ED SE ED SE ED SE ED SE ED SE  RE RE RE//First coin
SD ST DN  SN SN SN SN  SE ED SE ED SE ED SE ED SE ED SE  RE RE RE//Second coin
SD ST DN  SN SN SN SN  SE ED SE ED SE ED SE ED SE ED SE  RE RE RE
SD ST DN  SN SN SN SN  SE ED SE ED SE ED SE ED SE ED SE  RE RE RE
SD ST DN  SN SN SN SN  SE ED SE ED SE ED SE ED SE ED SE  RE RE RE
SD ST DN  SN SN SN SN  SE ED SE ED SE ED SE ED SE ED SE  RE RE RE
SD ST DN  SN SN SN SN  SE ED SE ED SE ED SE ED SE ED SE  RE RE RE//7th Coin
```

To Generate the 25 seeds for each RAIDA:

RAIDA ID | DN |SN | Seed | Concatenated Hex Strings | SHA-384 Hash. Truncated to 16 bytes. 
---|---|---|---|---|---
0x00 | 0xFD | 0x009EF945  | 0xD427AC7B9EF945768E435E | 0x00FD009EF945D427AC7B9EF945768E435E | D9F0CC6D564E8425651BBC6925B0062E
0x01 | 0xFD | 0x009EF945  | 0xD427AC7B9EF945768E435E | 0x01FD009EF945D427AC7B9EF945768E435E | E32D6DC6B7A0A3272E2D8444BF9CDE1C
0x02 | 0xFD | 0x009EF945  | 0xD427AC7B9EF945768E435E | 0x02FD009EF945D427AC7B9EF945768E435E | C6C4D93385C22287C7647F6F2EFA9BE9

1. The software will take all the binary numbers (RAIDA ID, denomination, Serial Number, 11 byte seed ) and turn them into hexidecimal string that uses all uppercase characters. 
2. The resulting number 34 character string will generate a SHA-384 Hash.
3. The hash is trucated to 16 bytes and that becomes the binary AN number. 


# FORMAT 'B'
## Coin Header Codes for Format 'B'
For this standard, there is one seed for all the coins. Each coin is just the split number, Denomination number and the serial number. This gives us 6 bytes per coin. 

After the header, there is an 11 byte seed. THis keeps the same seed size as with format 'A':
 11 Byte Seed (SE ED) 
|---|
| SE ED SE ED SE ED SE ED SE ED SE | 

Then there are six bytes for every coin. 

Shard | Denomination 1 Byte | Serial Number (4 Bytes) 
---|---|---
SD | ST | DN | SN SN SN SN  



##  Sample file body in format 'B'
This has 7 coins in it. Each coin takes up 20 bytes. 
```
CC CC CC //Coin Count (How many coins are in the file)
SE ED SE ED SE ED SE ED SE ED SE //Seed
SD ST DN  SN SN SN SN  // First coin
SD ST DN  SN SN SN SN  
SD ST DN  SN SN SN SN 
SD ST DN  SN SN SN SN  
SD ST DN  SN SN SN SN 
SD ST DN  SN SN SN SN  6th Coin
```

To Generate the 25 seeds for each RAIDA (Same as with 'A'):

RAIDA ID | DN |SN | Seed | Concatenated Hex Strings | SHA-384 Hash. Truncated to 16 bytes. 
---|---|---|---|---|---
0x00 | 0xFD | 0x009EF945  | 0xD427AC7B9EF945768E435E | 0x00FD009EF945D427AC7B9EF945768E435E | D9F0CC6D564E8425651BBC6925B0062E
0x01 | 0xFD | 0x009EF945  | 0xD427AC7B9EF945768E435E | 0x01FD009EF945D427AC7B9EF945768E435E | E32D6DC6B7A0A3272E2D8444BF9CDE1C
0x02 | 0xFD | 0x009EF945  | 0xD427AC7B9EF945768E435E | 0x02FD009EF945D427AC7B9EF945768E435E | C6C4D93385C22287C7647F6F2EFA9BE9

1. The software will take all the binary numbers (RAIDA ID, denomination, Serial Number, 11 byte seed ) and turn them into hexidecimal string that uses all uppercase characters. 
2. The resulting number 34 character string will generate a SHA-384 Hash.
3. The hash is trucated to 16 bytes and that becomes the binary AN number. 



