# Encryption and Data Protection

## 1. What is Encryption?

Encryption is the process of converting readable data (plaintext) into an unreadable format (ciphertext) using an encryption algorithm and a cryptographic key.

Only someone with the appropriate decryption key can recover the original data.

Example:

```text
Plaintext
   |
   v
Encryption Algorithm + Key
   |
   v
Ciphertext
   |
   v
Decryption Algorithm + Key
   |
   v
Original Plaintext
```

## 2. What is Encryption at Rest?

Encryption at rest protects data stored on physical or virtual storage systems.

Examples:
- Database records.
- Files stored on disks.
- Cloud object storage.
- Database backups.
- Archived files.

Common techniques:
- AES-256 encryption.
- Database encryption.
- Disk-level encryption.
- File-level encryption.
- Encrypted backups.

Example:

```text
Application
     |
     v
Database
     |
     v
Encryption at Rest
     |
     v
Encrypted Storage
```

The objective is to protect stored information if the underlying storage is accessed without authorization.

## 3. What is Encryption in Transit?

Encryption in transit protects data while it travels between two systems over a network.

Examples:
- Browser to web server.
- API Gateway to backend service.
- Microservice to microservice.
- Application to database.

Common technologies:
- TLS (Transport Layer Security).
- HTTPS.
- Mutual TLS (mTLS).

Example:

```text
Client
   |
   | HTTPS / TLS
   |
   v
API Gateway
   |
   | TLS
   |
   v
Backend Service
   |
   | TLS
   |
   v
Database
```

TLS protects communication against network interception and unauthorized modification.

## 4. Encryption at Rest vs Encryption in Transit

| Feature | At Rest | In Transit |
|---|---|---|
| Protects | Stored data | Data moving across networks |
| Location | Disk, database, storage | Network communication |
| Common technology | AES-256 | TLS |
| Example | Encrypted database | HTTPS API request |
| Objective | Protect stored information | Protect communication |

## 5. What is Encryption in Use?

Encryption in use refers to techniques that protect data while it is actively being processed.

Examples:
- Confidential computing.
- Trusted Execution Environments (TEEs).
- Hardware-based secure enclaves.
- Homomorphic encryption.

Protecting data during computation is more challenging than protecting stored or transmitted data.

## 6. What is Symmetric Encryption?

Symmetric encryption uses the same secret key for encryption and decryption.

Example:

```text
Plaintext
    |
    v
Secret Key
    |
    v
AES Encryption
    |
    v
Ciphertext
    |
    v
Same Secret Key
    |
    v
AES Decryption
    |
    v
Plaintext
```

Common algorithm:
- AES (Advanced Encryption Standard).

AES key sizes:
- AES-128
- AES-192
- AES-256

Advantages:
- Fast.
- Efficient for large amounts of data.

Disadvantage:
- The secret key must be securely shared and managed.

## 7. What is Asymmetric Encryption?

Asymmetric cryptography uses two mathematically related keys:

1. Public Key.
2. Private Key.

Data encrypted using a recipient's public key can be decrypted using the corresponding private key.

Example:

```text
Sender
   |
   v
Recipient's Public Key
   |
   v
Encryption
   |
   v
Ciphertext
   |
   v
Recipient's Private Key
   |
   v
Decryption
   |
   v
Original Data
```

Common algorithms:
- RSA.
- Elliptic Curve Cryptography (ECC).

Advantages:
- Supports secure key establishment.
- Supports digital signatures.

Disadvantage:
- More computationally expensive than symmetric encryption for large data volumes.

## 8. Symmetric vs Asymmetric Encryption

| Feature | Symmetric | Asymmetric |
|---|---|---|
| Keys | One shared secret key | Public and private keys |
| Speed | Faster | Generally slower |
| Example | AES | RSA, ECC |
| Typical use | Encrypting large data | Key establishment and digital signatures |

## 9. What is TLS?

TLS stands for Transport Layer Security.

It is a security protocol that protects communication between systems over a network.

HTTPS uses TLS to secure web communication.

### Simplified TLS handshake

1. Client connects to the server.
2. Client and server negotiate supported TLS parameters.
3. Server presents its digital certificate.
4. Client validates the server certificate.
5. Both sides establish shared session keys.
6. Encrypted communication begins.

Modern TLS uses asymmetric cryptography for authentication and key establishment, followed by symmetric encryption for application data.

## 10. What is mTLS?

mTLS stands for Mutual Transport Layer Security.

In regular TLS, the client validates the server's identity.

In mTLS:
- Client validates the server certificate.
- Server validates the client certificate.

Both parties authenticate each other using certificates.

Example:

```text
Service A
    |
    | Client Certificate
    |
    v
Service B
    |
    | Server Certificate
    |
    v
Mutual Authentication
    |
    v
Secure Communication
```

Common use cases:
- Microservice communication.
- Internal APIs.
- Service-to-service security.

## 11. What is Hashing?

Hashing converts input data into a fixed-length output called a hash.

Hashing is designed to be a one-way operation.

Examples:
- SHA-256.
- SHA-512.

Example:

```text
Input: Hello World
       |
       v
    SHA-256
       |
       v
Fixed-length hash
```

Common use cases:
- Data integrity verification.
- Digital signatures.
- Password verification when using appropriate password hashing algorithms.

For password storage, use Argon2id, bcrypt, or scrypt with appropriate parameters rather than plain SHA-256.

## 12. Encryption vs Hashing vs Encoding

| Feature | Encryption | Hashing | Encoding |
|---|---|---|---|
| Purpose | Confidentiality | Integrity / verification | Data representation |
| Reversible | Yes, with key | No, designed to be one-way | Yes |
| Uses keys | Yes | Usually no | No |
| Example | AES | SHA-256 | Base64 |

Important: Base64 is encoding, not encryption.

## 13. What is a Digital Signature?

A digital signature verifies the authenticity and integrity of data.

The sender signs data using a private signing key.

The recipient verifies the signature using the corresponding public key.

Typical process:

1. Sender calculates a cryptographic hash of the data.
2. Sender signs the hash using a private key.
3. Recipient receives the data and signature.
4. Recipient verifies the signature using the public key.
5. If verification succeeds, the recipient can establish that the data has not been modified and was signed by the holder of the corresponding private key.

## 14. What is Key Management?

Key management is the process of securely generating, storing, distributing, rotating, and revoking cryptographic keys.

Important concepts:
- Key generation.
- Secure key storage.
- Key rotation.
- Key expiration.
- Key revocation.
- Key backup and recovery.
- Access control.

Common key management solutions:
- AWS KMS.
- Azure Key Vault.
- Google Cloud KMS.
- Hardware Security Modules (HSMs).

Encryption is only as secure as its key management practices.

## 15. What is Envelope Encryption?

Envelope encryption uses two levels of encryption.

1. Data Encryption Key (DEK): Encrypts the actual data.
2. Key Encryption Key (KEK): Encrypts the DEK.

Example:

```text
Original Data
     |
     v
Data Encryption Key (DEK)
     |
     v
Encrypted Data

DEK
 |
 v
Key Encryption Key (KEK)
 |
 v
Encrypted DEK
```

The KEK is managed by a secure key management service.

This approach helps manage encryption keys without requiring the master key to directly encrypt all application data.

## 16. What is Key Rotation?

Key rotation means periodically replacing encryption keys with new keys.

Benefits:
- Limits the impact of compromised keys.
- Supports security policies.
- Reduces long-term exposure.

During rotation, systems must maintain access to existing encrypted data until it has been safely re-encrypted or its previous keys are no longer needed.

## 17. What is PGP Encryption?

PGP stands for Pretty Good Privacy.

It provides encryption and digital signatures for files and messages.

Common use cases:
- Secure file transfers.
- Protecting sensitive information.
- Encrypting files before SFTP transfers.

Example:

```text
Source File
    |
    v
PGP Encryption
    |
    v
Encrypted File
    |
    v
SFTP Transfer
    |
    v
PGP Decryption
    |
    v
Original File
```

SFTP protects the file transfer channel, while PGP can protect the file contents independently of the transfer channel.

## 18. What are Common Data Protection Best Practices?

1. Encrypt sensitive data at rest.
2. Use TLS for data in transit.
3. Use strong encryption algorithms.
4. Securely manage cryptographic keys.
5. Rotate keys according to security policies.
6. Apply least-privilege access controls.
7. Avoid storing passwords in plaintext.
8. Mask or tokenize sensitive information where appropriate.
9. Encrypt database backups.
10. Maintain audit logs and monitor access.

## 19. How do Encryption and Access Control Work Together?

Encryption protects data confidentiality, while access control determines who is allowed to access resources.

Example:

1. A user authenticates.
2. The application checks the user's permissions.
3. The application retrieves encrypted data.
4. The authorized application decrypts the data using controlled access to the encryption key.
5. The user receives only the information they are authorized to access.

Both encryption and authorization are necessary for a secure application.

## 20. Quick Revision

| Concept | Key Point |
|---|---|
| Encryption at Rest | Protects stored data |
| Encryption in Transit | Protects network communication |
| Encryption in Use | Protects data during processing |
| AES-256 | Symmetric encryption |
| RSA | Asymmetric cryptography |
| TLS | Secure network communication |
| HTTPS | HTTP over TLS |
| mTLS | Mutual certificate authentication |
| SHA-256 | Cryptographic hashing |
| Digital Signature | Authenticity and integrity |
| KMS | Key management service |
| HSM | Hardware Security Module |
| Envelope Encryption | DEK encrypted by KEK |
| PGP | File and message encryption |
| Key Rotation | Replacing cryptographic keys |

## 21. Data Protection Across the Data Lifecycle

A secure application should protect data throughout its lifecycle:

- At rest: Encryption.
- In transit: TLS.
- In use: Confidential computing where required.
- Access: RBAC and ABAC.
- Identity: OAuth 2.0 and OIDC.
- Integrity: Hashing and digital signatures.
- Key protection: KMS and HSM.
