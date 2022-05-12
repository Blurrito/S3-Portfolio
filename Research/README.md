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

### What sort of security does the Nintendo DS use
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

## What prevents a Nintendo DS from making a secured connection?
When a Nintendo DS attempts to establish a secure connection, it will validate the certificate the server returns. In order for the Nintendo DS to accept the certificate as legitimate, it has to be signed by an intermediate certificate (and it's accompanying private key) that can be traced back to the Nintendo intermediate certificate via the chain of trust. As no authentic server certificate/private key combination is not publicly available, it is impossible to have a Nintendo DS accept a secured connection to a server other than Nintendo's official servers.

## What solutions are available?
In order to force the Nintendo DS to accept connections to these domains, two countermeasures have been created.

### Editing the executable of a game
The first solution involves altering the executable of a game. By altering all mentions of `https` to `http`, the Nintendo DS will attempt to connect to all servers using unsecured connections. Using this method means no security implementation is necessary on the server's end. This makes setting up a server significantly easier to do. It also allows for all domain names to be altered, allowing Nintendo DS systems to directly connect to a domain owned by the server owner without the need of an altered DNS list.

However, this method also comes with a few disadvantages. First, the game gets stripped of what little security was implemented, making it an even bigger security threat. Second, due to the nature of how Nintendo DS cartridges work, the data on them cannot be overwritten. This means that legitimate cartridges cannot connect to the server using this method. Finally, additional hardware is needed to play the altered game files. Said hardware is deemed illegal in most countries, and thus not easily obtainable, making this an investment.

#### Setup process

#### Production example
The image below shows a network capture of a Nintendo DS connecting to `nas.nintendowifi.net` without any modifications. As shown, the Nintendo DS and the server perform the handshake prodecure before exchanging encrypted messages with one another.
![With SSL](https://i.imgur.com/8NO8AoM.png)

The image below uses the same situation as before. The difference being that the game has been altered as to not use secured connections. Instead of initiating the handshake procedure, the game instead opens up an unsecured TCP connection. As shown, the message the Nintendo DS sends to the server is in plain text.
![Without SSL](https://i.imgur.com/zF4h8XP.png)

### Abusing a flaw in the certificate validation process.
The second solution involves abusing a flaw in the Nintendo DS certificate verification process. When the Nintendo DS verifies the server certificate, it attempts to follow the chain of trust back to the Nintendo intermediate certificate. However, it does not verify if all of the certificates in the chain are allowed to act as an intermediate. This means that any certificate/private pair that can be traced back to the Nintendo intermediate certificate can be used to generate a server certificate that the Nintendo DS will accept. This method would mean no modifications are necessary on the client side for this method to work. This also means legitimate cartridges will be able to connect to the server using this method.

This method too comes with a few disadvantages. First, this method would require the server owner to set up a custom DNS list, as the requested domain names do not exist anymore. Second, this method requires a certificate/private key pair that can be used to generate a server certificate. Finally, knowledge on how to generate a server certificate and implement it in a server is necessary to get the server running, which might be difficult due to the lacking documentation on the topic.

#### Certificate generation process
The following process is a recollection of steps I went through to generate a server certificate the Nintendo DS will accept. The following will be assumed:
- The reader is in possession of a working OpenSSL installation.
- The reader is in posession certificate/private key pair that can be traced back to Nintendo's intermediate certificate. This may be combined in `p12` format or split in `.csr` format for the certificate and `.key` format for the private key.
- The target machine runs a copy of Windows. Some parts might differ with other operating systems.

Furthermore, the certificate/private key combination will be referred to as `NWC.p12` from here on out.


**1.** If the certificate/private key pair has already been separated in `.csr` and `.key` files, start at step 4. If not, it first needs to be split. To do so, use the following command:

`openssl pkcs12 -in NWC.p12 -passin alpine -passout alpine -out keys.txt`

**2.** If the command completed successfully, a keys.txt file should have been created the folder where the `NWC.p12` file is located. Open this file in a text editor, and create a new file in a hex editor (HxD is recommended). Select everything from the start of the first line to the end of the first `----CERTIFICATE END----` line (including the blank line underneath), copy it and paste it in the text segment of the new file in the hex editor. Finally, Save this file as `NWC.crt`.

**3.** Create another new file in the hex editor and perform the same actions as with the `NWC.crt` file, but use everything underneath the previously selected segment instead. Save this file as `NWC.key`

**4.** Once `NWC.csr` and `NWC.key` have been created, they can be used to generate a new server certificate. To start this process, use the following command to generate a 1024-bit RSA key for the new certificate:

`openssl genrsa -out server.key 1024`

- If the command has finished running successfully, a new file named `server.key` should have appeared in the same directory as the `NWC.p12` file. If it has, use the following command to generate a new certificate using the `server.key` file generated in the previous step:

`openssl req -new -key server.key -out server.csr`

During this process, a series of questions regarding the organization the certificate belongs to will be asked. The table underneath shows the values used by the official server certificate (and the certificate I generated), but can be customized if desired.
| Question                 | Answer                                                 |
|--------------------------|--------------------------------------------------------|
| Country Name             | US                                                     |
| State or Province        | Washington                                             |
| Locality name            |                                                        |
| Organization name        | Nintendo of America Inc.                               |
| Organizational unit name | Nintendo CA                                            |
| Common name              | Domain name or `*.*.*` for all subdomains              |
| Email address            | ca@noa.nintendo.com                                    |
| Challenge password       | Can be anything (`alpine` recommended for consistency) |
| Optional company name    |                                                        |

**5.** If the command has finished successfully, a new file named `server.csr` should have appeared in the same directory as the `NWC.p12` file. This is the new server certificate that will be used by the server to authenticate with the Nintendo DS. Before it can be used, it first needs to be signed by the `NWC.csr` certificate file and `NWC.key` key file. To do so, use the following command:

`openssl x509 -req -in server.csr -CA NWC.crt -CAkey NWC.key -CAcreateserial -out server.crt -days 3650 -sha1`

This certificate will be valid for 3650 days, or 10 years. This value may be changed to suit the needs of your server

**6.** Once the newly created server certificate has been signed, the certificate and private key pair can be combined into one file for installation in the certificate store. To do so, use the following command to combine the two into a .pfx file:

`openssl pkcs12 -export -inkey server.key -in server.crt -out server.pfx`

When prompted to use an export password, anything can be used. However, `alpine` is recommended for consistency.

**7.** If the command has finished successfully, a new file named `server.pfx` should have appeared in the same directory as the `NWC.p12` file. In order to install both certificates to the certificate store, double-click on both the `NWC.p12` and `server.pfx` files and going through the installation wizard for both. When asked for an export password, use `alpine` for the `NWC.p12` certificate and the password entered during the previous step for the `server.pfx` certificate. When asked to select the certificate store, select `Place all certificates in the following store` and select the `Personal` store. Once this has been done, the certificates are ready for use in whichever environment the user requires them.

#### Production example
Underneath is a code snippet containing a simple TCP listener with SSL authentication and certificate search functionality. The program starts by setting up the TCP listener and searching for the required certificates. This is done by providing the names of those who the certificate was issued to (`*.*.*` for the server certificate, `Wii NWC Prod 1` for the intermediate certificate). After a successfull setup, the program starts listening for incoming connections.

Once a pending connection request has been detected, the server accepts the connection before retrieving the data stream and authenticating with the client. As noted earlier in this document, the server uses SSLv3 as the cryptographic protocol and provides the client with both the server certificate and the certificate chain. By providing both the server certificate and certificate chain, the Nintendo DS will be able to follow the chain of trust back to the Nintendo intermediate certificate and accept the connection.

```c#
using System.Net;
using System.Net.Security;
using System.Net.Sockets;
using System.Security.Authentication;
using System.Security.Cryptography.X509Certificates;
using System.Text;

namespace NdsSslConnection
{
    class Program
    {
        private static TcpListener _Listener;
        private static X509Certificate2 _Certificate;
        private static X509Certificate2Collection _CertificateChain = new X509Certificate2Collection();

        static void Main(string[] args)
        {
            //Set up a TCP listener
            _Listener = new TcpListener(IPAddress.Parse("127.0.0.1"), 443);
            _Listener.Start();

            //Get the server certificate and intermediate certificate
            GetServerCertificate("*.*.*");
            GetAdditionalCertificate("Wii NWC Prod 1");

            while (true)
            {
                //Wait for an incoming connection
                if (_Listener.Pending())
                {
                    //Accept the incoming connection
                    TcpClient Client = _Listener.AcceptTcpClient();

                    //Authenticate with the client and get the stream
                    SslStream ClientStream = GetStream(Client);

                    //Wait for data to come in
                    do { Task.Delay(100); }
                    while (Client.Available == 0);

                    //Read data
                    byte[] Request = new byte[Client.Available];
                    ClientStream.Read(Request, 0, Request.Length);

                    //Print request to console
                    string ConvertedRequest = Encoding.UTF8.GetString(Request);
                    Console.WriteLine(ConvertedRequest);

                    //Disconnect and clean up
                    ClientStream.Close();
                    Client.Close();
                    Client.Dispose();
                }
                else
                    Task.Delay(100);
            }
        }

        /// <summary>
        /// Gets the server certificate associated with this TCP listener instance.
        /// </summary>
        /// <param name="CertificateName">The name of the certificate to search for.</param>
        /// <param name="StoreName">The name of the store to look in.</param>
        /// <param name="StoreLocation">The location of the store to look in.</param>
        static void GetServerCertificate(string CertificateName, StoreName StoreName = StoreName.My, StoreLocation StoreLocation = StoreLocation.CurrentUser) => _Certificate = GetCertificate(CertificateName, StoreName, StoreLocation);

        /// <summary>
        /// Gets additional certificates associated with this TCP listener instance and adds these in the certificate chain.
        /// </summary>
        /// <param name="CertificateName">The name of the certificate to search for.</param>
        /// <param name="StoreName">The name of the store to look in.</param>
        /// <param name="StoreLocation">The location of the store to look in.</param>
        static void GetAdditionalCertificate(string CertificateName, StoreName StoreName = StoreName.My, StoreLocation StoreLocation = StoreLocation.CurrentUser) => _CertificateChain.Add(GetCertificate(CertificateName, StoreName, StoreLocation));

        /// <summary>
        /// Gets a X509 certificate by name from the provided store name and location.
        /// </summary>
        /// <param name="CertificateName">The name of the certificate to return.</param>
        /// <param name="StoreName">The name of the store to search in.</param>
        /// <param name="StoreLocation">The location of the store to search in.</param>
        /// <returns></returns>
        /// <exception cref="KeyNotFoundException">No certificate in the provided store matches the provided name</exception>
        private static X509Certificate2 GetCertificate(string CertificateName, StoreName StoreName, StoreLocation StoreLocation)
        {
            X509Store Store = new X509Store(StoreName, StoreLocation);
            Store.Open(OpenFlags.ReadOnly);

            try
            {
                X509Certificate2Collection StoreCollection = Store.Certificates;
                X509Certificate2Collection FoundCertificates = StoreCollection.Find(X509FindType.FindBySubjectName, CertificateName, false);
                if (FoundCertificates.Count < 1)
                    throw new KeyNotFoundException($"Could not find a certificate with name { CertificateName }. Make sure this certificate has been added to the selected certificate store (Store name: { StoreName }, Store location: { StoreLocation }).");
                return FoundCertificates[0];
            }
            finally
            {
                Store.Close();
            }
        }

        /// <summary>
        /// Authenticates with the client and sets up a stream.
        /// </summary>
        /// <param name="Client">The client who to authenticate with.</param>
        /// <returns>The authenticated SSL stream.</returns>
        private static SslStream GetStream(TcpClient Client)
        {
            SslStream EncryptedStream = new SslStream(Client.GetStream(), false);
            EncryptedStream.AuthenticateAsServer(new SslServerAuthenticationOptions
            {
                ServerCertificateContext = SslStreamCertificateContext.Create(_Certificate, _CertificateChain),
                EnabledSslProtocols = SslProtocols.Ssl3,
                ClientCertificateRequired = false
            });
            return EncryptedStream;
        }
    }
}
```

### Conclusion

## Sources
