---
title: CMU 95702 - Cryptography
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-10-10 15:21:45
---
CMU 95702 - Distributed System

Cryptography
<!-- more -->

***

## Symmetric key

### Julius Caesar (Shift) cipher
1. Pick a key (a number)
2. Shift the letters of the plaintext by the key to create the ciphertext.
3. E.g.
    – Plaintext: Yellow cake 
    – Key: 3
    – Ciphertext: Bhoorz fdnh
4. Secret key algorithm
    – The sender and the receiver share a secret key
5. Symmetric algorithm
    – Trivially-related keys are used to encode and decode the message
    - Trivially-related: uses the same key, or keys require only a simple transformation.

### Block Cipher
1. Symmetric key cipher 
2. Works on blocks of text
    – E.g. 128 bit blocks

```
初始块 01001
key   00001
原文   00011000010101100101

    00011 00001 01011 00101 
xor 01001                   \\ 与初始块做XOR
=   01010                   
+ k 00001                   \\ 加上key
=   01011

xor       01011             \\ 上一轮的输出作为这一轮的作XOR的块
=         01010
+ k       00001             \\ 加上key
=         01011

xor             01011
=               00000
+ k             00001
=               00001

xor                   00001
                      00100
+ k                   00001
=                     00101 

    01011 01011 00001 00101
```

Block cipher chaining:
1. Each block is XOR’ed with the previous block before encrypting
    –  The first block is XOR'ed with an initialization vector
    –  Thein initialization is typically a randomn number
    用一个随机得到的初始块作为第一轮的作异或的块
2. Therefore each block is dependent on all plaintext blocks up to that point


## Asymmetric key
1.  Uses asymmetric keys 
2.  Single public key 公钥
    – Let the world see 大家都知道你的公钥
3.  Single private key 密钥
    – You keep secret 只有你自己知道你的密钥


### Twos of Public/Private key
1. **Encryption / Decryption:** 加密解密
    - 别人用你的公钥加密消息，传给你
    - 你收到密文以后，用自己的私钥解密。
2. **Signing / Verification:**
    - 你用自己的私钥对文档进行加密
    - 别人用你的公钥对密文进行解密，然后验证文档是不是你当初签名过的文档。

## Cryptographic protocols

**protocols:**
1. Cryptographic protocols build on the use of cryptographic algorithms 
加密协议是建立在加密算法之上的
2. Two prominent protocols: 
    – Transport Layer Security (TLS) (newer) 
    – Secure Socket Layer (SSL) (older)
3. Both are application-level protocols, that work above the transport layer (especially TCP) to provide safe end-to-end communication. 
两种协议都是在传输层之上

**Secure Email & Web:**
1. *SMTP* and *IMAP* (email protocols) can work above TLS or SSL to provide secure email transmission
两个邮件协议，也是通过TLS或者SSL来提供邮件加密的。
2. HTTPS is HTTP over TLS/SSL to provide secure web communication

**Comparation:**
1. Symmetric:
    –  Fast 加密算法更快，效率高
    –  Difficult to distribute and and keep keys secure 但是怎么让所有人都有对称密钥是个麻烦的问题。
2. Asymmetric:
    –  100 to 1000 times slower 非对称加密要慢100倍以上
    –  Can allow for public keys 
3. TLS / SSL uses the best of both worlds:
    –  Use asymmetric keys to exchange symmetric keys at the beginning of a conversation。 
    先用非对称加密把对称密钥传到所有人手中。
    –  Symmetric keys will have the lifespan of that conversation
    之后就能用对称加密来加密了。

## TLS/SSL Protocal HandShake
1. Start with RSA Public/Private keys 
    –  Ask Amazon for their Public key
    –  Amazon replies with their Public key
    向亚马逊要到公钥
2. Generate a 128 bit(or bigger) random number
    – This is your "session" new AES key 
    这个是对称加密的对称密钥，这个对称密钥是随机的，只对这次session有用
    – Encrypt the new AES key with the Amazon Public Key 
    用公钥把对称密钥加密
    – Send the encrypted key to Amazon
    把密文传给亚马逊
3. Amazon receives the encrypted key
    –  They (and only they) can decrypt the AES key with their RSA Private key
    亚马逊用自己的私钥对密文进行解密，然后就能得到对称加密要用到的对称密钥。
    –  They now have the AES key you created for this session
    这样大家收发双方都有对称加密的对称密钥了
4. Amazon and your browser communicate by encrypting and decrypting all messages with the same AES 128 bit random-number key.
    –  At the end of this session, both forget the AES key
    这次session结束以后，这个对称密钥就可以退休了。

## Digital Signature
**Digital Signature**
1. Public key encryption can be used to provide digital signatures to validate authenticity 公钥加密可以被用来做数字签名
2. Digital signatures are better that real signatures, for people can alter a paper document once you have signed it. 
3. With digital signatures, if the document is changed, then the signature becomes invalid.
4. This is because the signature is a number based on the content of the document.
5. Or more specifically, on the hash value of a document.

**Hash Function:** 得到原文的哈希值
1. Takes a file (application, document, picture, etc.)
2. Returns a large, but fixed-size number. (Hash)
3. The file being encoded is called the “message”
4. The resulting hash value is also called the 
    – "Message Digest"
    – Or simply "Digest"

一个好的hash funciton：
1. 对于一个hash value来说，不可能造出一个新的message与之相同
2. 如果改变了原文，那么hash value 也一定会改变
3. 两个不同的原文，不能得到相同的hash value


**Digital Signature**
The sender:
1. Take your document (email message, etc)
1. Calculate a hash function on it.
    – e.g. SHA2
1. Encrypt the resulting hash value with your private key.
对哈希值用私钥进行加密
1. The encrypted hash value is the digital signature.
加密得到的结果就是数字签名
1. Send it ...

The recipient:
1. Receives the document (email message, etc) and the digital signature.
2. Decrypts the signature with public key 
用公钥对数字签名进行解密
3. Calculate a hash of the document & compare it with the sender’s hash value
对原文做哈希
4. They should be equal
比较哈希的结果和解密的结果来验证

**Digital Cetificates**
还有一个不安全的地方是，我们向亚马逊要公钥，我们怎么知道发来公钥的这个人到底是不是亚马逊。这个时候就要数字证书这个东西。
1. A Digital Certificate is a document that provides information about an organization
    – Most importantly, its public key
2. And the Digital Certificate is digitally signed by some trusted party.
数字证书是可信的第三方签发的
3. Typically contains
    –  Owner’s name
    –  Owner’s public key
    –  Expiration date
    –  Name of certificate issuer –  Serial number
    –  Issuer’s digital signature

**Certification Authority**



## Cryptographic Protocols 
A protocol is a series of steps, involving two or more parties, designed to
accomplish a task.
协议就是一系列的步骤，用于多方一起来完成同一个任务

cryptographic protocol：
A cryptographic protocol is a protocol that uses cryptography
加密协议就是用到加密算法的协议（Orz 真是跪了

### Some Attacks and an Assumption 
**攻击的种类：**
1. Eavesdropping or listening in 窃听
2. Masquerading or pretending to be someone you are not 身份伪装
3. Tampering by a man in the middle 改动消息
4. Replaying of old messages 可以在中间看到message，拦截，然后迟一些再传
5. Denial of service 拒绝服务

**参与者：**
1. Alice: First participant
2. Bob: Second participant
3. Trent: A trusted third party
4. Carol: Participant in three-party protocols
5. Eve: Eavesdropper
6. Mallory: Malicious attacker
7. Sara: A trusted server

**标记**
1. $K_{A}$ Alice’s key that she keeps secret.
2. $K_{B}$ Bob’s key that he keeps secret.
3. $K_{AB}$ Secret key shared between Alice and Bob
4. $K_{Apriv}$ Alice’s private key (known only to Alice in asymmetric key crypto)
5. $K_{Apub}$ Alice’s public key (published by Alice for all to read)
6. $\\{M\\}_K$ Message M encrypted with key K
7. $[M]_K$ Message M signed with key K

<br>
***
<br>

## Four Important Cryptographic Protocols

### Scenario 1 (Like WWII & TEA)
就是对称加密
1. Communication with a shared secret key.
2. Alice and Bob share $K_{AB}$. 
双方都已经知道对称密钥了
3. Alice computes $E(K_{AB},M_i)$ for each message $i$. 
加密 
4. She sends these to Bob.
5. Bob uses {% raw %}$D(K_{AB}, \{M_i\} K_{AB})${% endraw %} and reads each $M_i$. 
解密

***

### Scenario 2 (Like Kerberos)
设计三方的一种安全认证系统

Alice想要和Bob进行加密沟通

1. Alice asks Sarah for a ticket to talk to Bob.
2. Sarah knows Alice’s password so she can compute $K_A$. 
这个$K_A$，Sarah和Alice都知道。Sarah是全知视角。这个$K_A$也就是Alice的密码，可以看做Alice和Sarah共享的对称密码。
3. Sarah send to Alice {% raw %}$\{\{Ticket\}K_B,K_{AB}\}K_A${% endraw %}
4. Alice knows her password and is able to compute {% raw %}$K_A${% endraw %}.
所以Alice可以解密，之后得到了可以供A，B加密的对称密钥$K_{AB}$ 和 加密好的$\\{Ticket\\}K_B$
5. Alice sends a read request to Bob. She sends **$\\{Ticket\\}K_B$,Alice,Read**. Another challenge!
6. Bob uses $K_B$ to read the content of the Ticket.同样$K_B$也是Bob和Sarah之间共享的对称密钥
7. The Ticket is $K_{AB}$,Alice. Bob and Alice then use this session key to communicate.

Does not scale well : Sarah must know $K_A$, $K_B$ ....
Sarah is a single point of failure.

***

### Scenario 3 (Authentication)

Alice wishes to convince Bob that she sent the message M. 
Alice要发一个经过签名的文档给Bob

1. She computes a digest of $M$, $Digest(M)$.
2. If the Digest method is a good one, it is very difficult to find another message $M’$ so that $Digest(M) == Digest(M’)$.
2. Alice makes the following available to the intended users: $M,{Digest(M)}K_{Apriv}$.
3. Bob obtains the signed document, extracts $M$ and computes $Digest(M)$.
Bob decrypts ${Digest(M)}K_{Apriv}$ using KApub and compares the
result with his calculated $Digest(M)$. 
4. If they match, the signature is valid.

另外
MAC algorithm is a symmetric key cryptographic technique to provide message authentication. For establishing MAC process, the sender and receiver share a symmetric key $K$.
MAC 算法是就是就是运用对称加密的方式来进行签名的方法，这里用 A B 共有的 session key， 而不是非对称的密钥。
Message Authentication Codes (MACs) 就是hash以后的结果。

***

### Scenario 4 (Like SSL)
这个就是先RSA 在 DEA（先非对称，再对称）

***

### keystore/truststore
一种解释：
A **keystore** contains private keys, and the certificates with their corresponding public keys.

A **truststore** contains certificates from other parties that you expect to communicate with, or from Certificate Authorities that you trust to identify other parties.

顾名思义：keystore 记录的是你的 key 和证书，而 trust store 记录的是你信任的证书，一般是对方服务器的证书(当然也可能是 SSL 客户端证书，比如金融交易中要求客户端也必须有证书)。

***

更详细的解释：
In a SSL handshake the purpose of trustStore is to verify credentials and the purpose of keyStore is to provide credential.

**KeyStore**:
keyStore in Java stores *private key* and *certificates* corresponding to their public keys and require if you are SSL Server or SSL requires client authentication.

**TrustStore**:
TrustStore stores certificates *from third party*, your Java application communicate or certificates signed by CA(certificate authorities like Verisign, Thawte, Geotrust or GoDaddy) which can be used to identify third party.

**TrustManager**:
TrustManager determines whether remote connection should be trusted or not i.e. whether remote party is who it claims to and KeyManager decides which authentication credentials should be sent to the remote host for authentication during SSL handshake.

***

If you are an SSL Server you will use private key during key exchange algorithm and send certificates corresponding to your public keys to client, this certificate is acquired from keyStore. 

On SSL client side, if its written in Java, it will use certificates stored in trustStore to verify identity of Server. SSL certificates are most commonly comes as .cer file which is added into keyStore or trustStore by using any key management utility e.g. keytool.

***

Over!
