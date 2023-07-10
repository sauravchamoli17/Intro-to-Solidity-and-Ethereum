## Practical cryptography in Ethereum

### What is cryptography?
- Cryptography is the branch of mathematics that ensures the confidentiality and authenticity of information.
- Cryptography lies in Ethereum roots and makes up a descent part of its operability. Ethereum highly utilizes hash functions, public key cryptography, and digital signatures. Cryptography is used to control Ethereum users’ funds, process transactions, maintain Ethereum state, etc.

### What is a hash function?
- A hash function is a function of converting an array of input data of arbitrary length into an output bit string of fixed length, which is performed using a certain algorithm.

### Hash functions in Ethereum
- Keccak256 (sha-3 family), used pretty much everywhere in the platform. From the construction of addresses and tx hashes, to the hash function of Merkle-patricia state tree.
- Sha256 (sha-2 family), available as a precompile at address(2).
- Ripemd160, available as a precompile at address(3).

### What is a cryptographic key?
- A cryptographic key is a digital sequence of a certain length, created according to certain rules, using random (or pseudo random) number generators and / or calculated according to a special algorithm from other values.

### Public key cryptography in Ethereum

![ECDSA](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688822015/Screenshot_from_2023-07-08_18-43-15_a7mu2r.png)

- Ethereum uses elliptic curve public key cryptography.
- The secp256k1 elliptic curve is used.
- The EOA private key is a unique account identifier.
- Smart contracts do not have private keys.
- A seedphrase is an entropy to the private keys generator.

### How an Ethereum EOA address is generated?

- Generate a private key k.
- Multiply k by the elliptic curve generator point G to get the public key K.
- Concatenate x and y coordinates of K.
- Calculate H the keccak256 hash of the concatenation.
- Take 20 the least significant bytes of H. This will be an EOA address.
- The address may be “hidden” behind an ENS name.

### What is a digital signature?
- A digital signature is an analogue of a handwritten signature that provides two properties: the ability to verify the authenticity and integrity of the document, which protects it from modification and substitution.

### ECDSA and Ethereum

- Ethereum uses the RFC-6979 standard of ECDSA to make signatures deterministic without security compromises. The determinism is achieved by providing the keccak256 hash of the concatenation of the private key with the message as an entropy to ECDSA.
- ECDSA outputs 3 parameters: V, R, S. Where R and S are the parts of the signature and V is a recover identifier (27 or 28).
- EcRecover precompile is available at address(1).

![Signature](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688822311/Screenshot_from_2023-07-08_18-48-20_vcmrct.png)

### Three standards of Ethereum signatures

- The vanila ECDSA, where message hash gets signed, used for signing transactions.
- The [personal sign](https://eips.ethereum.org/EIPS/eip-191), where to the original message the prefix “\x19Ethereum Signed Message:\n” + len(message) is added.
- The [typed sign](https://eips.ethereum.org/EIPS/eip-712), where the typed message is elaborately constructed to mitigate the signature replays.

### EcRecover Usage

- When EOA constructs the transaction there is no “from” field.
- The EOA signs the transaction via their private key using ECDSA.
- When transaction is sent, the VRS parameters together with raw transaction bytes are broadcast to the nodes.
- Courtesy of ECDSA, the nodes recover the EOA address from VRS and raw transaction bytes.
- Ether from the recovered EOA is deducted to pay for gas. 

### Signatures and Solidity

- Implementing secure signature verification in Solidity is hard. There are many attack vectors to consider.
- Rule of thumb is if signatures are involved, they should cover the most parameters.
- User signatures must be EIP-712 compliant.
- Never use ecrecover precompile directly.

[Deterministic Usage of the Digital Signature Algorithm (DSA) and Elliptic Curve Digital Signature Algorithm (ECDSA)
](https://datatracker.ietf.org/doc/html/rfc6979?authuser=0)
