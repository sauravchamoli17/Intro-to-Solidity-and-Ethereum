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
