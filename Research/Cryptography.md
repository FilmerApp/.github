# Cryptography
Many of the applications you use nowadays collect a great deal of personal data: not only names and passwords, but sometimes also phone numbers, addresses or other info regarding your personal life. Data leaks are common, and proper protection of user data is vital for any application.

One of the most important defenses against data being stolen is encryption. Encryption can and should be used on almost every level of an application, and - when strong enough - ensures that even if data is intercepted somewhere along its route, it is useless to whoever captured it.

The OWASP top 10 lists ‘Cryptographic Failures’ as its number 2 security flaw, describing it as 

> “(...) failures related to cryptography (or lack thereof).”
> 

The high position on this list illustrates its importance, which is why it is essential to examine cryptographic options for any application you develop.

In this research, I want to investigate how my own application, Filmer, and the application we made with our group, Eeventify, use encryption to secure data, and what cryptographic aspects could be added or improved to increase their security.

# Sensitive Data

Before deciding on our methods, we need to decide on what to decrypt in our applications. While it never hurts to protect everything, some data is much more important and personal to users, and that is where our focus should lie. The EU’s General Data Protection Regulation considers the following data sensitive:

- personal data revealing racial or ethnic origin, political opinions, religious or philosophical beliefs;
- trade-union membership;
- genetic data, biometric data processed solely to identify a human being;
- health-related data;
- data concerning a person’s sex life or sexual orientation.

Luckily both of my applications don’t store any of this data, but that does not mean they don’t contain any sensitive data whatsoever. A good example of data that could be sensitive is passwords, not only because obtaining those would offer access to our own applications, but also because many people reuse passwords, which means that one leaked password could be used to access other locations.

# Data At Rest

Data at rest refers to any place data is stored: not only databases, but also the cloud or simply a hard drive. Both of my applications use a database, which, if not encrypted, can be a security risk: if someone manages to get their hands on the database they will be able to access its data freely.

The data in our database should be encrypted, and should only be decrypted when an authorised user needs to access it. The encryption should use a sufficiently strong method, otherwise it can still easily be decrypted in the case of a leak. The exact mathemathical implementation of the different cyphers are (by necessity) incredibly complex and are outside the scope of this research, but some good examples include the Advanced Encryption Standard (AES), a standard used by the U.S. Government amongst others, or RSA, which is older and relatively slow.

Column level encryption is also an option; instead of encrypting the entire database, this technique allows you to encrypt specific information or attributes within a database.

A good option for my own applications would be Transparent Data Encryption (TDE), a technology developed by Microsoft that is part of Microsoft SQL Server. This would be easy to implement, but still keep the database secure.

One thing of note is that these types of encryption methods generate a key that can be used to decrypt the data. It is important to make sure this key stays secure, seperate from the encrypted data, and is updated on a regular basis.

Just encryption our database will not be enough to secure my applications however: when I read data from the databases it automatically gets decrypted, so when the data starts getting moved around it is vulnerable.

# Data at Transit

This category is slightly larger: the data needs to pass through the backend, be transferred to a user over the internet, and different data might need to be sent back, and there are multiple security risks along this path.

We established that passwords are sensitive data, so lets take those as an example. When a user logs in, they enter their password in plaintext and it gets sent to the backend to be authorized. Currently, in Filmer, passwords are stored unencrypted in the database, retrieved from it, and compared to the password recieved from the user. This is obviously a security flaw, and Eeventify handles it slightly better, by hashing the password in the backend.

Unlike the database encryption methods mentioned before, hashing is one-way: once a password is hacked, it is virtually impossible to reverse the process. There are many hashing functions with differing levels of security - ideally I want to one that is salted, meaning random data is added to the input of a function to guarantee a unique output. This way, even when 2 users choose the same password, their hash will be different, and it protects the passwords from dictionary attacks. Good examples of salted hashing functions include scrypt or Argon2.

Hashing further secures our password, but we have to deal with one last glaring issue: the encryption currently only happens in my backends. When a user enters a password, it is sent to the server over the internet, where it could potentially be intercepted before it can ever be hashed.

Both of my applications use HTTP, which is insecure. A better option would be to switch to HTTPS, which uses Transport Layer Security (TLS) to encrypt its communication, authenticate the involved parties, and ensures the sent data has not been tampered with. When a user connects to a TLS connection a handshake begins, during which the user’s device and the server will authenticate the server’s identity, specifiy which version of TLS will be used, and most imporantly generate session keys that are used to encrypt all further messages after the handshake is complete.

To use HTTPS, a server needs a TLS certificate (often also called SSL certificate, after TLS’s predecessor). Part of this certificate is the server’s public key, which can be used to unscramble data to ensure the server’s authenticity. This data, however, can only be encrypted with  the private key used by the server, making sure no outside sources can change the data.

# Conclusion

There are quite a few steps that could still be taken to ensure the various data used in my applications is securely encrypted: encrypting the database, hashing passwords, and ensuring a TLS certificate for the servers.

Encryption is a complex and ever-evolving field: even if an application conforms to encryption standards one year, it might be very different five years later. Encryption techniques get replaced by more sophisticated ones, or, in a worse case, get ‘solved’ rendering them nearly useless. Choosing the right techniques to fit your application and ensuring those techniques are up to date is essential.

### Sources

OWASP Top 10 item on cryptographic failures - [https://owasp.org/Top10/A02_2021-Cryptographic_Failures/](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)

GDPR definition of personal data - [https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/legal-grounds-processing-data/sensitive-data/what-personal-data-considered-sensitive_en](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/legal-grounds-processing-data/sensitive-data/what-personal-data-considered-sensitive_en)
