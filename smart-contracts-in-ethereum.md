## Smart Contracts in Ethereum

### What is a smart contract?
- In Ethereum terms, smart contracts are immutable decentralized programs that run deterministically in the context of EVM. They are neither smart nor legal contracts, but the term has stuck.
- Smart contracts are usually written in a high-level language like Solidity or Vyper and then compiled to bytecode.
- Smart contracts are used to bring real world concepts and value to Ethereum.

### Life cycle of a smart contract
- Smart contract is designed, implemented and compiled to bytecode.
- Smart contract is extensively tested and audited.
- Smart contract is deployed via a special transaction and its address is determined.
- Users atomically interact with a smart contract either directly or through DAPPs. The contract may in turn interact with other contracts.
- Smart contract may be destructed if it allows so. Deprecated feature.

### Smart contracts execution environment

![Smart Contract Execution Environment](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688905626/Screenshot_from_2023-07-09_17-56-54_b2ryvq.png)

### What is Solidity?

- Solidity is a contract oriented programming language created by Gavin Wood. The main product is a compiler, solc, which converts programs to the EVM bytecode. The latest version is 0.8.19.

```solidity
contract Example {
 event RecievedEther(uint256 amount);
 address public owner;
 
 constructor() {
   owner = msg.sender;
 }
 
 recieve() external payable {
   emit RecievedEther(msg.value);
 }
 
 function withdrawEther() external {
    require(msg.sender == owner, "Not an owner");
    (bool ok,) = msg.sender.call{value: address(this).balance}{""};
    require(ok, "Failed to send ether");
 }
}
```

### How do DAPPs interact with SC?

- Dapps interact with smart contracts through function calls and event listening. They invoke functions defined in the contract using the ABI, which specifies function names, parameters, and return types. dApps can also retrieve contract data and respond to events emitted by the contract. This interaction enables dApps to execute functions, access contract state, and provide real-time updates based on contract events.
