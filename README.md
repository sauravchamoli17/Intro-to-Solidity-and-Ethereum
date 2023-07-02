# Intro to Solidity & Ethereum

This repository contains notes from a course named [Basic Aspects of Programming with Solidity](https://distributed.education/solidity-course) from [Distributed Lab](https://distributedlab.com/).

## Table of Contents

- [Ethereum the world computer](#ethereum-the-world-computer)
- [Ethereum transactions and blocks](#ethereum-blocks)
- [Practical cryptography in Ethereum](#cryptography)
- [Ethereum smart contracts](#smart-contracts)
- [Solidity Variables](#solidity-variables)
- [Solidity Functions](#solidity-functions)
- [OOP in Solidity](#solidity-oop)
- [Advanced Solidity: Assembly and data locations](#advanced-solidity)
- [Proxy and libraries](#proxy-libraries)
- [Advanced Solidity: Precompiles, signatures, and bytecode](#precompiles)
- [Token standards, ERC20 tokens](#token-standards)
- [ERC721 and ERC1155 tokens](#erc721-erc1155)

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
## Practical cryptography in Ethereum
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

