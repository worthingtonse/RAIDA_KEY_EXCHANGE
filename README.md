# RKE ( RAIDA Key Exchange )
How to shares keys in a quantum safe manner. To make the protocol extensible and flexible, there are many different ways that things can be done. 
I will try to add some discussion about this 

There are many standards an protocols within this standard:

[Application Host File Aquisition](README.md#application_host_file_aquisition)

[Directory Protocol](README.md#directory_protocol)

[Key Server Protocol](README.md#key_server_protocol)

[Key Client Protocol](README.md#key_client_protocol)


# Application Host File Aquisition
As a program, I want to know the addresses of key servers so I can swap keys. 

>People using the same program could atomatically get the same key servers.

> 

Programs, such as web browsers, chat, file sharing etc, need to know where key exchange servers are. This creates a vulnerability because
DNS sysems could tell the wrong address and key servers can be hacked. 

To make things more secure there are differetn ways: 

Use a lookup service
  --Yes? 
        DNS or IP Address
        RAIDA or Client Server
  --No?
      Hardwire IP addresses?
      or Hardwire FQDNs?
      
Is the list of servers to be used private, members only or Anonymous 

1. Hardwire IP addressess of the information servers who know where the key servers are (Guardian Approach)

2. Have a list of FQDNs that tell the program where they can find lists of key servers. (Root hints Approach)

3. Provide a "host" file that can be edited. 

# Directory Protocol
As a user, I want to post the contact information to the key servers that I am using so that others can send me keys.

Post one computer or post an entire RAIDA?

# Key Server Protocol
As a user, I want to post keys to the key servers. As a user, I want to download keys. I also want to be able to 


# Key Client Protocol
As a program, I want to be able to put together keys in the correct way. I also need to be able to use the correct encryption and all that jazz. 
