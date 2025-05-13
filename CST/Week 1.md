12/05/25

- Business assets are protected from unauth access, modification and deletion
- Assets are available to auth users and systems when required
- Assets can be recovered in a timely manner when compromised
- Assets are protected in a manner that complies with regulatory and/or industry standards
- Assets are assessed and protected according to risk
- Involves everyone in the organization


#### Optus Breach

- 9.8M customers impacted
- 2.8M passports and license details exposed
- 37K Medicare numbers exposed
- $1.5B loss in brand
- Unsecured API endpoint was exposed pubically that allowed anyone, no auth
- API facilitated access to very sensitive backend customer data
- Incrementing customer identifiers were used; these differed by a sequence of 1. Highly predictable
- Vulnerable second domain, not in use but online, contained coding error that was fixed in the primary domain but not a second

#### MediBank Breach

- 9.7M user impacted
- 200GB data extracted including customer data, medicare claims
- Admin details stored on a personal device of a third-party IT provider - device infected with malware
- Remote access did not enforce MFA allowing attackers to access systems
- Attackers installed a script to search and extract sensitive customer data
- Data was exfiltrated via built-in backdoor
- Security tools had been employed which detected suspicious activity but were not acted on in a timely manner


#### EQUIFax

- Credit reporting agency
- 145M people impacted
- Data extracted including birth dates, physical addresses and SSN
- Approx 200,000 people had their credit card numbers stolen
- Stolen data never publicized on the dark web

### Confidentiality Integrity Availability (CIA)

- C - Ensure data and services are only accessed by auth pricipals
- I - Data and services are accurate and complete
- A - Always are available to auth principals
- Viewed as a checklist guide around tools and techniques
- Used to consider how different sec controls may interact, i.e. CIA are all linked

#### Crypto 101

- Crypto - protecting information by mathematically transforming it such that its confidentiality integrity and auth can be assured and verified
- Cipher - A math algo that specifies a transformation
- Encrypt - Apply a cipher to transform data
- Decrypt - Apply cipher to recover plain text
- Plaintext - Original data

- Asymmetric algos that use two mathematically related keys
- Relatively slow
- Primarily used for key exchange
- Not used for bulk encryption

- RSA - Factor large numbers into prime factors, min: 2048
- ECC - Elliptic Curve Theory, min: 224
- DSS - Discrete Logarithm problem, min: 2048

- Symmetric ciphers - algos that used the same key to encrypt and decrypt
- Random seq of bytes of various lengths
- Key length in tied to the cipher
- Symmetric keys are typically referred to as a secret keys
- Security of the ciphers is related to the key length; the longer the key the better
- Much faster than asymmetric
- Need to share the key in advanced
- All symmetric ciphers process data in blocks with the block size related to the cipher

- CBC - Start with XORing the first block with an IV (init vector), then XORing subsequent blocks with the previous block
- GCM - Take each block and encrypt individually, encrypt the counter instead of the data. XOR the counter with the original data. Optional authentication that can be included into the payload to ensure data integrity (i.e. auth tag, need to be kept to decrypt - 0^128 -> Encrypt with private key -> XAND with AAD)


- Read the NIST document - take not on how often the keys should be refreshed

#### Hashing

- Take data of arbitrary length and compress it down to a fixed length
- Avalanche - Small changes to the input should result in large changes to the output
- No way to predict the output
- Should avoid clustering
- SHA-1 should not be used, SHA-2 current standard
- SHA-256 most widely used
- SHA-3 latest in the family and mostly widely used
- Plaintext -> Hash Function -> Digest
- Not to be used for data integrity


### MACs

- A crypto technique that applies an auth mechanism to the output of a hash function
- Prevents the hash from being regenerated


### Digital signatures

- A mechanism for verifying the authenticity of a document
- Digital signatures usually comprise:
	- A digest
	- Asymmetric encryption of the digest using the private key

- Exam - know how to use the different encryption modes, when to use. Not implementation details. No questions on breaches.

- TLS - the most dangerous code in the world, watch/read