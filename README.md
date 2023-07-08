# Notes on Solidity & Ethereum

This repository contains notes from [Basic Aspects of Programming with Solidity](https://distributed.education/solidity-course) course from [Distributed Lab](https://distributedlab.com/).

## Table of Contents

- [Ethereum the world computer](#ethereum-the-world-computer)
- [Ethereum transactions and blocks](#ethereum-transactions-and-blocks)
- [Practical cryptography in Ethereum](#practical-cryptography-in-ethereum)
- [Ethereum smart contracts](#ethereum-smart-contracts)
- [Solidity Variables](#solidity-variables)
- [Solidity Functions](#solidity-functions)
- [OOP in Solidity](#oop-in-solidity)
- [Advanced Solidity: Assembly and data locations](#advanced-solidity-assembly-and-data-locations)
- [Proxy and libraries](#proxy-and-libraries)
- [Advanced Solidity: Precompiles, signatures, and bytecode](#advanced-solidity-precompiles-signatures-and-bytecode)
- [Token standards, ERC20 tokens](#token-standards-erc20-tokens)
- [ERC721 and ERC1155 tokens](#erc721-and-erc1155-tokens)

## Ethereum the world computer

### What is Ethereum?

- Ethereum is a POS(Proof of Stake) blockchain with 500K+ validators. 1 million transactions happens daily on ethereum and during bull run events it is upto 10 million. It has community of thousands of researchers and developers.

- Ethereum has EVM which is turing complete. Turing completeness means a system or language can perform any computation, simulate other systems, and has loops, conditionals, unbounded memory, and arbitrary precision.

- Solidity and Vyper are popular high-level programming languages specifically designed for Ethereum smart contract development. In contrast, the Ethereum Virtual Machine (EVM) operates at a lower level and directly executes low-level instructions called opcodes.

- Ethereum's ecosystem encompasses a diverse range of on-chain and off-chain components, for example: The Graph which provides a robust infrastructure for decentralized innovation and data indexing on the Ethereum network.

- In the Ethereum network, every node maintains a complete copy of the Ethereum Virtual Machine (EVM), enabling independent verification and execution of transactions and smart contracts.

### Why the world computer?

![World Computer](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688286185/Screenshot_from_2023-07-02_13-46-47_cz68gn.png)

- Within the Ethereum network, the state encompasses Ethereum balances, the state of smart contracts, transaction receipts, and logs that smart contracts emit, forming a comprehensive record of the network's financial and computational activities.

- During a transaction in Ethereum, the state of the system undergoes changes, but it may not immediately reach its final state due to the asynchronous nature of the network and the need for consensus among nodes.

- The number of transactions a block can hold in Ethereum depends on the gas limits, with an average throughput of 15-20 transactions per second.

### Ethereum basics

- Ether serves as the native currency of the Ethereum blockchain, while transaction gas acts as a unit representing the computational complexity of transactions. Ethereum features externally owned accounts for individuals and smart contracts, which are programmable entities that can execute arbitrary on-chain programs.

- The maximum gas limit for a block in Ethereum is set at 30 million gas, while the cheapest transaction, such as transferring ethers, costs 21,000 gas.

- The transaction cycle in Ethereum involves sending a transaction, deducting the gas limit from your account, and upon execution completion, receiving a gas refund that returns any unused gas back to you.

- Gas price in Ethereum is determined by a specialized algorithm introduced in the London hardfork, which incorporates the base fee of the block and an optional tip provided by the sender as compensation for the validator.

- When the Ethereum network observes that the blocks are consistently exceeding the 15 million gas threshold, the base fee for transactions will increase as a mechanism to manage network congestion and prioritize transaction processing.

### Ethereum vs Bitcoin

![ETH VS BITCOIN](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688287657/Screenshot_from_2023-07-02_14-17-18_nsb1om.png)

- In Ethereum, each epoch consists of 32 slots, and once an epoch is executed and completed, a new epoch begins, ensuring the finality of blocks and maintaining the progress of the blockchain.

- When 2/3 or more of the validators in Ethereum accept the validity of an epoch, it achieves a state of safety. Once a new epoch is added on top of the existing one, the epoch becomes finalized, providing assurance and permanence to the blockchain.

### Some Facts About Ethereum

![Facts About Ethereum](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688288506/Screenshot_from_2023-07-02_14-31-15_vwdjga.png)

## Ethereum transactions and blocks

### What is a transaction?

- A transaction is an atomic action inititated by EOA that either changes the ethereum state or reverts.

![Transaction](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688805482/Screenshot_from_2023-07-08_14-07-05_gr31go.png)

### Types of Transaction

- Regular transactions that send ETH from one account to another (cheapest transaction).
- Contract deployment transactions where 'to'is address(0) and data is contract creation bytecode.
- Transaction that interact with an already deployed smart contract where 'to' is address of the smart contract and execution payload is in data field.

### Transaction Fields

- Nonce: Special counter that ethereum maintains, to keep count of EOA transactions initiated.
- Max priority fee per gas: Max price of a validator tip per consumed gas (London hardfork).
- Max fee per gas: Base Fee + Priority Fee
- Gas Limit: max amount of gas the transaction can consume.
- Recipient: destination ethereum address.
- Value: amount if ETH(wei) sent to the recipient.
- Data: When interacting with the smart contract, it is ABI encoded execuion payload. When deploying a contract data is bytecode of the contract.
- v,r,s : ECDSA signature of the EOA.

### Facts About Transaction

- EIP 2718 introduced a "transaction envelope" to support future transaction types (users to specify which type of transaction they are sending).
- chain id is included in the RLP encoding to mitigate transaction replays on the other EVM chains (chain id for ethereum mainnet is 1).
- Transaction hash is a keccak256 hash of its signed RLP counterpart.

### Transaction lifecycle
- Transaction is constructed, signed, and RLP encoded.
- Transaction hash is calculated.
- Transaction is broadcast to the network and added to the mempool.
- Validator picks a transaction from the mempool and includes it in a block.
- Block gets added to the chain and the transaction executes.

### What is a block?

-  Blocks are batches of transactions mainly needed for the optimization purposes.
![Blocks](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688808411/Screenshot_from_2023-07-08_14-56-38_kree0r.png)

### Inside a block

- The Ethereum block is a complex structure that contains both the intricate PoS consensus information and the transactions execution payload.
- Execution payload is contained within the new PoS consensus.
- The list of transactions.

![Execution Layers](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688809108/Screenshot_from_2023-07-08_15-08-13_srqi0s.png)

### Execution Layer Structure

![Execution Layer](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688809549/Screenshot_from_2023-07-08_15-15-08_n5rqgd.png)

### Block lifecycle

- Block validator is pseudorandomly but deterministically chosen via PoS consensus algorithm.
- Validator assembles the block as he wishes to maximize profit (MEV).
- Validator executes block transactions one by one and sends the block to peers.
- At the end of the epoch attestation on block validity begins.
- Block gets marked as safe.
- After another epoch block gets marked as finalized.

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

## Ethereum smart contracts
## Solidity Variables
## Solidity Functions
## OOP in Solidity
## Advanced Solidity: Assembly and data locations
## Proxy and libraries
## Advanced Solidity: Precompiles, signatures, and bytecode
## Token standards, ERC20 tokens
## ERC721 and ERC1155 tokens

## Resources

List additional resources such as books, tutorials, online courses, or other repositories that could be useful for learning Solidity and Ethereum.

## Contributing

If you would like to contribute to this repository, please follow the guidelines in [CONTRIBUTING.md](CONTRIBUTING.md).

## License

This repository is licensed under the [MIT License](LICENSE).

