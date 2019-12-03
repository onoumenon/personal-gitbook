# Public key infrastructure

{% hint style="info" %}
Resources used: 

[YouTube video explaining PKI concepts](https://www.youtube.com/watch?v=t0F7fe5Alwg)

[TLS Security](https://www.acunetix.com/blog/articles/tls-security-what-is-tls-ssl-part-1/)
{% endhint %}

## PKI

PKI \(or Public Key Infrastructure\) is the framework of encryption and cybersecurity that protects communications between the server \(your website\) and the client \(the users\). It works by using two different cryptographic keys: a public key and a private key. The public key is available to any user that connects with the website. The private key is a unique key generated when a connection is made, and it is kept secret. When communicating, the client uses the public key to encrypt and decrypt, and the server uses the private key. This protects the user’s information from theft or tampering. \(via [Venafi](https://www.venafi.com/education-center/pki/how-does-pki-work)\)

![Source: https://www.youtube.com/watch?v=t0F7fe5Alwg](../.gitbook/assets/image%20%2818%29.png)

The message encrypted with the public key must be decrypted by the private key pair. Since you do not want to be sending sensitive information to bad/ wrong agents, digital certificates offers authentication of the server/ user you are communicating with.

Digital certificates are issued by Certificate Authorities \(like VeriSign, Comodo and GlobalSign\), and has a bunch of information packaged with them. 

![](../.gitbook/assets/image%20%2816%29.png)

Most modern browsers checks for the security of the website you are visiting. It checks the Certificate Authority against a group of trusted CAs \(going through the trust chain to find the Root Certificate Authority\), as well as its signature.

Certificates can be revoked for a variety of reasons, and certificate owner/ admin can request for a reissue.

![Source: https://www.youtube.com/watch?v=t0F7fe5Alwg](../.gitbook/assets/image%20%2823%29.png)

