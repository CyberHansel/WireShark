Decrypting SSL Traffic
Handshake loosely follows this format:

    The client sends a list of availble cipher suites it can use along with a random set of bytes referred to as client_random
    The server sends back the cipher suite that will be used, such as TLS_DHE_RSA_WITH_AES_128_CBC_SHA, along with a random set of bytes referred to as server_random
    The client generates a pre-master secret, encrypts it, then sends it to the server.
    The server and client then generate a common master secret using the selected cipher suite
    The client and server begin communicating using this common secret

Decryption Requirements

There are several ways to be able to decrypt traffic.

    If you have the client and server random values and the pre-master secret, the master secret can be generated and used to decrypt the traffic
    If you have the master secret, traffic can be decrypted easily
    If the cipher-suite uses RSA, you can factor n in the key in order to break the encryption on the encrypted pre-master secret and generate the master secret with the client and server randoms

* Analyze --> Expert Information  #overview of what is happening in the packets analyzed

* (http.request or ssl.handshake.type == 1) and !(udp.port eq 1900)       #HTTP and initial HTTPS traffic
* (http.request or ssl.handshake.type == 1 or tcp.flags eq 0x0002) and !(udp.port eq 1900)   #HTTP and initial HTTPS traffic + TCP SYN
* (http.request or ssl.handshake.type == 1 or tcp.flags eq 0x0002 or dns) and !(udp.port eq 1900)      #HTTP and initial HTTPS traffic + TCP SYN + DNS requests
