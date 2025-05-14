
Hash algorithms
- Mathematical functions that transform data of any size to a fixed length string
- The transformation is one-way. That is, you cannot derive the original message from the digest.
- Information loss transformation

Digest
- The string produced by a hashing algorithm

Message Authentication Code (MAC)
- Prevents the hash from being regenerated and provides authentication and better integrity protection
- MACs use an underlying hash algorithm but introduce a secret key / password

Common MAC Algorithms
- Hash-based Message Authentication Code or HMAC that is combined with the SHA-2 or SHA-3 hash function families.
- E.g. HMAC-SHA256 and HMAC-SHA3-256

