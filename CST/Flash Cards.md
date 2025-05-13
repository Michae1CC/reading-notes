
Cipher

- A mathematical algorithm that specifies a transformation.

Ciphertext
- The original data after encryption; that is after the cipher has been applied.

Asymmetric Encryption
- Asymmetric or public-key ciphers are cryptographic algorithms that use two mathematically related keys:
	- One key is called the public key, the other the private key.
	- The public key is shared with anyone, and the private key is kept secret.
	- Data encrypted with the public key can be decrypted with the private key, and conversely data encrypted with the private key can be decrypted with the public key.

Asymmetric ciphers
- RSA - Factor very large numbers into their prime factors.
- ECC - Elliptic Curve Theory
- DSS - Discrete Logarithm problem.

Block cipher
- A symmetric-key cryptographic algorithm that transforms one block of information at a time using a cryptographic key.
- For a block cipher algorithm, the length of the input block is the same as the length of the output block.

Public-key certificate
- A set of data that uniquely identifies an entity, contains the entity’s public key and possibly other information, and is digitally signed by a trusted party, thereby binding the public key to the entity

Trust anchor
- An authoritative entity for which trust is assumed
- The self-signed public key certificate of a trusted CA

Certification authority
- The entity in a Public Key Infrastructure (PKI) that issues certificates to certificate subjects.

Public Key Infrastructure (PKI)
- A framework that is established to issue, maintain, and revoke public-
key certificates.

RSA Key Sizes
- 512 – 8192
- NIST Min: 2048

ECC Key Sizes
- 192 – 521
- NIST Min: 224

DSS
- 512 – 4096
- NIST Min: 2048


Symmetric ciphers
- Symmetric ciphers are cryptographic algorithms that use a shared key to both encrypt and decrypt
- Symmetric key length is tied to the cipher and are normally 128, 192, and 256 bits in length; 256 bits is the current recommended length.
- All symmetric ciphers process data in blocks with the block size related to the cipher