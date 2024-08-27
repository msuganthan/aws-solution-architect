### Encryption in Flight (TLS/SSL)

* Data is encrypted before sending and decrypted after receiving
* TLS certificates help with encryption(HTTPS)
* Encryption in flight ensures no MITM(man in the middle attack) can happen

<img src="../images/aws-security-encryption/encryption-in-flight.png" alt="Encryption in flight">

### Server-side encryption at rest

* Data is encrypted after being received by the server
* Data is decrypted before being sent
* It is stored in an encrypted form thanks to a key(usually a data key)
* The encryption / decryption key must be managed somewhere, and serer must have access to it.

<img src="../images/aws-security-encryption/server-side-encryption.png" alt="Server side encryption">

### Client-side encryption

* Data is encrypted by the client and never decrypted by the server
* Data will be decrypted by a receiving client
* The server should not be able to decrypt the data
* Could leverage Envelope Encryption

<img src="../images/aws-security-encryption/client-side-encryption.png" alt="Client side encryption">