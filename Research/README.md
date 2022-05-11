# Communication & Security
## Table of contents

## Introduction
This research paper is a combination of two separate research topics, each attempting to answer a one of two questions. These documents have been concatonated for the readers' ease of access and the close relation between the two topic.

The first document aims to answer the question of "**What sort of security measures does the Nintendo DS use while communicating with the server?**". In order to answer this question, one must first understand how communcation between two computers, as well as the implementation of security functions.

The second document aims to answer the question of "**How can one make the Nintendo DS accept a secured connection?**" by using the foundation laid down in the first research paper. In order to fully understand this topic, one must understand and know the security measures implemented in the Nintendo DS, as well as the flaws present.

While a brief explanation on various topics of interest will be provided in this document, it is recommended to read through the provided source material for a more complete understanding of said topics.

## What sort of security measures does the Nintendo DS use while communicating with the server?

### How do computers communicate between one another?
To allow computers to communicate with one another, a tangible connection between the devices needs to be created. This is called a computer network: a collection of computers that are connected to one another. Computer networks can vary widely in size and structure, and can either be private or publicly available.

Each device present in a computer network is called a network node, and can be either an endpoint or redistribution point. Endpoints are, as the name suggests, sources of or destinations for network communication, and can be anything, ranging from desktop PC’s to smart fridges. Redistribution points are devices that are responsible for passing on the message send by an endpoint. Devices such as routers, modems, network switches and interfaces can be considered redistribution points.

In order for the devices in a network to communicate with one another, various types of protocols stacked on top of one another are used. These stacks are appropriately called protocol stacks, and are in most cases created in accordance to the Open Systems Interconnection (OSI for short) model. In a protocol stack, each layer (protocol) makes use of the functions of the layer underneath. This repeats until the lowest layer of the stack, which directly makes use of the hardware. Protocol stacks created according to the OSI model consist of seven layers: physical, data link, network, transport, session, presentation and application. 

### How are communcations between computers secured?
If not specified, communications between computers will be unsecured. This would allow for a plethera of security breaches to happen, including (but not limited to) man-in-the-middle attacks and identity forgery. In order to prevent such security breaches from happening, cryptography protocols are used to protect connections and messages.

One of the best-known cryptography protocols is SSL, otherwise known as **Secure Socket Layer**. It intends to provide security through the encryption of messages, validating the integrity of arrived messages, and authenticity of both parties through the use of certificates. It's successor, known as Transport Layer Security (TLS), is the current industry standard. At the time of writing, the current version is TLS v1.3.

#### Authentication procedure
When the client requests a connection to be made using SSL, the client and server will perform a process called a handshake. During this process, the client and server verify each other’s identity using the other’s certificate, select a cipher suite to use for the cryptography and generate a session key for encrypting/decrypting the data. If, at any point during this process, something unexpected happens, the handshake will fail and the connection will be rejected. Following is a visual representation of the handshake:

![SSL/TLS handshake](https://d1smxttentwwqu.cloudfront.net/wp-content/uploads/2015/07/SSLTLS_handshake.png)

#### Certificates
In order to validate a server’s authenticity, each server must have a digital certificate that proves that they are who they claim they are. These server certificates are issued by certificate authorities (CA for short): a trusted entity that is responsible for signing digital certificates. Certificates contain information about the server it was issued to, as well as the certificate authority that issued it and a digital signature. It may also contain the public key: the key used by the client to encrypt the session key during the handshake procedure. The server decrypts this session key using it’s own private key, which should never be shared.

Certificates operate in tree-like structures. At the base of this structure is the root CA certificate: a self-signed certificate that identifies a certificate authority. It is capable of signing new certificates, and say whether or not they can do the same during this process. If this is allowed, the resulting certificate becomes an intermediate CA certificate: a certificate that is allowed to sign certificates of it’s own in the name of the certificate authority. Each signed certificate inherits the trustworthiness of the certificate that signed it. This will create a chain that leads back to the root CA certificate, appropriately called the chain of trust. An image visualizing the chain of trust can be found here.

![Certificate chain of trust](https://upload.wikimedia.org/wikipedia/commons/thumb/0/02/Chain_Of_Trust.svg/1920px-Chain_Of_Trust.svg.png)

### Nintendo DS specifications
During communications between the Nintendo DS and the server, the Nintendo DS connects to a variety of different domains. Between these domains, a variety of protocols is used. Following is a list containing a selection of these domains, the type of socket it uses, application protocol it uses and whether or not it’s secured using SSL.

| Domain address                   | Socket type | Protocol type         | Uses SSL |
|----------------------------------|-------------|-----------------------|----------|
| conntest.nintendowifi.net        | TCP         | HTTP                  | No       |
| nas.nintendowifi.net             | TCP         | HTTP                  | Yes      |
| dls1.nintendowifi.net            | TCP         | HTTP                  | Yes      |
| \*.available.gs.nintendowifi.net | UDP         | GameSpy Matchmaking   | No       |
| \*.master.gs.nintendowifi.net    | UDP         | GameSpy Matchmaking   | No       |
| \*.ms\*.gs.nintendowifi.net      | TCP         | GameSpy Server Search | No\*\*   |
| gpcm.gs.nintendowifi.net         | TCP         | GameSpy Profile       | No       |
| gpsp.gs.nintendowifi.net         | TCP         | GameSpy Profile       | No       |

```diff
# *   = Value differs depending on the game
# **  = Server makes use of proprietary security measures
```

As the table above suggests, only a small selection of domains makes use of SSL to secure communications between the Nintendo DS and the server. While certain servers incoorporate their own security measures to secure the content of messages, the ones do not can be read as plain text. The following packet capture shows a message sent by the Nintendo DS to one of the servers (`gpcm.gs.nintendowifi.net`). As seen, the data within the packet is not encrypted and can be read as plain text.

![Nintendo DS unencrypted packet](https://i.imgur.com/XAcFzfJ.png)

In case the domain does make use of SSL security, the Nintendo DS will initiate the handshake procedure by sending a client hello message similar to the one shown in the image below. It also shows that the Nintendo DS makes use of SSLv3 as it's cryptographic protocol, as well as two available cypher suites, both using RSA with RC4. Finally, the Nintendo DS expects a server certificate that is signed by Nintendo's intermediate certificate, and will abort the connection attempt if not given or valid.

![Nintendo DS client hello](https://i.imgur.com/X8tzuoD.png)

### Conclusion
The Nintendo DS makes very little use of (SSL) security while communicating with the profile and multiplayer services. The servers that do make use of SSL security use the long since depricated SSLv3 and RSA with RC4 cipher suites. Of the servers that do not make use of SSL security, only a small number implements encryption of messages in any capacity, with the majority sending packets in plain text. While most services require authentication before they can be used, the algorithms for this process are public knowledge, making this insecure as well. While implementing the majority of servers would be possible without much trouble, there are severe security threads without a (feasible) solution.


## How can one make the Nintendo DS accept a secured connection?
Concerns
Security
Naturally, with the console being 18 years old at the time of writing this, it is to be expected that these specifications are antiquated. SSLv3 has been deprecated in 2015, and while it is still supported by modern operating systems, is only meant for legacy support. Both the WEP and WPA (v1) security protocols have been phased out, and are no longer supported by the majority of modems/routers. The RC4 cipher is no longer being supported by modern cryptographic protocols since 2015, after attacks on the algorithm started to weaken, or in some cases break, the RC4 in SSL/TLS. The cipher suite is disabled by default, but can be re-enabled through the registry, but this is highly discouraged. 
 
Establishing SSL connections
When the Nintendo DS requests a secured connection to be made, it will validate the authenticity of the server certificate. Not only do we not have possession of this server certificate, it cannot be used without the accompanying private key, as we cannot decrypt the session key the Nintendo DS generates without it. This effectively makes impersonating the services that require SSL impossible. While most of the services on the Nintendo DS use unencrypted data streams, the few services that do make use of SSL are integral to the system. This means that, without these services working as expected, the Nintendo DS will fail to connect to the Nintendo Wi-Fi Connection service. To circumvent this problem, there are two solutions:
Editing the ROM image of a game
This solution aims to ignore SSL altogether by modifying the ROM image of a game. This is done by changing the static URL’s in the game’s code to different servers. This will cause the Nintendo DS to connect to said server without SSL.
Using this method would completely remove the need to implement an SSL implementation on the server, making the server implementation considerably easier. There also exist tools that fully automate the replacement process, allowing users to make the necessary changes in an instant. On top of this, it also allows the domain name of the server to be customized.
However, due to the ROM image of a game needing to be modified, this process needs to be repeated for every single game. On top of this, the process will not work for legitimate retail cartridges, as these cannot be edited. Users would need a flash cart with Wi-Fi capabilities or a computer with sufficient hardware capabilities to emulate Nintendo DS games in order to play these edited ROM images.
Abusing a flaw
As described earlier in this paper, certificates can act as an intermediate if it is set to allow this during the signing process. Under normal circumstances, a certificate signed by an invalid intermediate results in an invalid certificate. However, the Nintendo DS does not validate that the certificate used as intermediate is allowed to do so. This means that any certificate signed by Nintendo’s certificate with the accompanying private key will produce a certificate that the Nintendo DS will accept as authentic.
This method would require very little work on the user’s end, only needing to set the DNS to the server’s IP address in the internet settings. It would also work on legitimate retail copies, as no modifications would need to be made to the ROM image of the game. However, this method would require significantly more experience in infrastructure and cybersecurity, and is difficult to properly implement without proper documentation or instructions.
