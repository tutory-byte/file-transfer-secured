# Secure File Transfer 
Secure File Transfer in Java using Socket Programming, RSA , AES.


A Java client-server system that allows users to download files from the server with end-to-end encryption and authentication.

### Client-server architecture, public and private keys 
[Working Code](mailto:pxyybpik6%40mozmail.com?subject=I%20am%20interested%20in%20the%20Code)


The system consists of a client and a server Java program, and they must be named Client.java and Server.java respectively. They are started by running the commands -

```java Server port```

```java Client host port userid``` -  specifying the hostname and port number of the server, the userid of the client.

1. The server program is always running once started, and listens for incoming connections at the port specified. When a client is connected, the server handles the request, then waits for the next request (i.e., the server never terminates). For simplicity, you can assume that only one client will connect to the server at any one time.
2. Each user has a unique userid, which is a simple string like alice, bob etc. The server has userid "server". Each user is associated with a pair of RSA public and private keys, with filenames that have .pub or .prv after the userid, respectively. Thus the key files are named alice.pub, server.prv, etc.

3. It is assumed that the server already has the public keys of all legitimate users, and each client program user already has their own private key as well as the public key of the server. They obtained these keys via some separate mechanism not described here, prior to the execution of the client and server programs, and is not part of these programs. The client and server programs never create any new RSA keys or distribute them.
4. All the key files are in the folder where the respective client/server program runs from. They must not be read from other folders. Your programs must not require keys that they are not supposed to have. The authentication and key agreement stage
5. When the client program starts, it connects to the server and sends it the userid of the client and 16 fresh random bytes, both encrypted with RSA/ECB/PKCS1Padding and with an appropriate RSA key so that only the server can decrypt it. It also generates a signature of these encrypted bytes using the SHA1withRSA signature algorithm, with the appropriate RSA key that proves the identity of this client. (RSA keys can also be used for signatures; in this system the same set of RSA keys are used for both encryption and signature purposes.) The encrypted contents and the signature are sent to the server.
6. Upon accepting the connection of a client and receiving the contents in the previous step, the server program decrypts them (using an appropriate RSA key) to obtain the client userid and those 16 bytes. It also verifies the signature (using an appropriate RSA key).
7. Next, the server generates its own 16 fresh random bytes, concatenates it after the client's 16 bytes, and encrypts the combined 32 bytes with RSA/ECB/PKCS1Padding and with a key so that only the corresponding client can decrypt it. It also generates a signature of the encrypted bytes using the SHA1withRSA algorithm and with a key that proves the identity of the server. The encrypted key and the signature are sent to the client.
8. The client receives these contents, verifies the signature (using an appropriate RSA key), and decrypts the encrypted bytes (using an appropriate RSA key) to obtain the 32 bytes. It checks whether the first 16 bytes are indeed those sent by the client itself initially; if not, it should terminate the connection (and do not continue with the rest of this protocol).
9. Both the client and server (independently) use these 32 bytes to generate a 256-bit AES key. Note that these unencrypted bytes (and the resulting key) must never be sent across the network. The file transmission stage
10. From now on, the client and server will take turns to send messages to each other: client to send requests, and the server to send responses. Unless otherwise specified, all these messages should be encrypted by the sender before sending, with AES/CBC/PKCS5Padding and the AES key agreed in the previous stage, and decrypted by the recipient upon receipt.
11. The CBC mode needs a 16-byte initialisation vector (IV); here we will hash the 32 bytes in the key agreement stage, using MD5, and take the resulting 16 bytes as the IV for the first message. Each subsequent message will use the MD5 hash of the previous IV as the new IV.
12. The client program repeatedly prompts the user to type in what they want to do. The user can type in one of three commands: "ls", "get filename", or "bye". The client program should send the command and the name of the requested file (if any) to the server. At least the filename should be encrypted; you can choose to encrypt the command itself or not. The server will react accordingly, as specified below.
13. If the command is "ls" (same as the linux command with same name), it is a request for the server to send a list of filenames of all files available for download. The server should send back the names of all files in its current working folder. But since the key files are also in the folder, it should filter out all ".prv" files and do not include any of them in the list. 
14. If the command is "get filename", it is a request for the server to send the contents of the file of that name. It reads and encrypts the content of that file and sends it to the client. The client decrypts the received content and saves it locally, with the same filename, in the current working folder. The requested file should be from the server's current folder, excluding any .prv files (i.e. what the client would see if they use the ls command). If it is not one of those files, it should notify the client that the file does not exist.
15. If the command is "bye", it means the client has no further requests. Both sides should end the connection. The client simply quits, whereas the server waits for the next client.

<meta name="google-site-verification" content="nHLxSjZlEonL-eySxS5El2TzqIeCQ-j4w8mtZnImrpQ" />
