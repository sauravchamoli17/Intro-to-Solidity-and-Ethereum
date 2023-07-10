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

[Deterministic Usage of the Digital Signature Algorithm (DSA) and Elliptic Curve Digital Signature Algorithm (ECDSA)
](https://datatracker.ietf.org/doc/html/rfc6979?authuser=0)

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

## Solidity Variables

### Solidity Variable Types

### Value types

- Value types in solidity are used inside the functions and mostly lives on stack.

```solidity
bool (aka uint8)
int8, int16, …, int256 (signed integers, 0xff..ff = -1)
uint8, uint16, …, uint256 (unsigned integers, 0x00..01 = 1)
address, address payable
bytes1, bytes2, …, bytes32 (0x10..00 = 1)
```

```solidity
contract Example {
   function foo() external {
       bytes4 selector = Example. foo. selector;
       address caller = msg. sender;
       uint256 callerUint256 = uint256(uint160 (caller));
       bool areEqual = ~(int256(0)) == int256(-1);
   }
}
```

### Memory types

- Special types of variables that consume more memory than stack allows, so they are placed on memory(heap).
- bytes (tightly packed raw bytes)
- string (tightly packed UTF-8 encoded characters)
- Fixed length arrays
- Dynamic length arrays

```solidity
contract Example {
    function foo() external {
        bytes memory someBytes = hex"112233445566";
        string memory someString = "Hello, world!";
        uint256[1 memory numbers = new uint256[1(10)];
        address [2] memory addresses = [address(1), address (2)];
        // Dynamic array of static arrays of strings
        string(2][] memory complexArray = new string[2][](5);
    }
}
```

### User-defined types
- lives mostly in memory with stack pointers, made by aggregation of several other value types or memory types.

```solidity
contract Example {
    struct User {
        string name;
        address account;
        uint32 age;
    }
    function fool() external {
        User memory user = User({name: "Bob", account: msg .sender, age: 223});
    }
}    
```

### Type aliases
- In Solidity, type aliases allow developers to define alternative names for existing types, making code more readable and providing a way to create custom types based on existing ones.

```solidity
type MyUint is uint256;
    library MyUinter {
        function add(MyUint a, MyUint b) internal pure returns (MyUint) {
            return MyUint.wrap(MyUint.unwrap(a) + MyUint. unwrap(b));
        }
    }
}    
```

### Fixed point number types

- Right now Solidity doesn’t support fixed point numbers, so to overcome the limitation simple trick is used. 
- The on-chain calculations are made on uint256 numbers with increased precision. When the result is needed, off-chain services query the contract and decrease the precision back. 
- So 2 / 100 would be 2 * <precision> / 100.

### 5 locations of Solidity variables

- Local variables (stack)
- Memory
- Calldata and returndata
- Storage
- Constants and immutables

### Stack
- Stack location refers to the memory location where variables and function parameters are stored during contract execution. It is a temporary storage area used for efficient processing and typically used for variables with a limited scope within a function.

### Memory
- Memory is a data location where smart contracts store temporary data. Memory is zeroed at the start and destroyed at the end of the call context. Maximal accessible memory pointer is 2**256-1.

![Memory](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688921406/Screenshot_from_2023-07-09_22-19-49_aw3u7s.png)

### Calldata and returndata

- `calldata`: It is an immutable and read-only area of memory that contains the function arguments and data sent from an external contract or externally owned account (EOA) when calling a function. Solidity functions can access and read data from calldata but cannot modify its contents.

- `returndata`: It is the memory area used to store the return values and data sent back by external function calls. When a contract invokes a function on another contract, the return data from that function call is stored in returndata. The calling contract can read and process the returned data.

### Storage
- Storage is a data location where smart contracts store their state. Storage is a map of 32-byte slots to 32-byte values. If anything is written to storage, it is retained past the completion of the transaction. Non-initialized slots return zero when read.

![Storage](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688921674/Screenshot_from_2023-07-09_22-24-20_cm7jze.png)

### Constants and immutables

- Constants and immutables are special types of variables that live in the contract’s bytecode. - Constants are (usually) evaluated during compilation and immutables during contract’s deployment. After that they are encoded directly in the bytecode where they are used.

```solidity
contract Example {
    address internal constant ADMIN = address (5);
    address internal immutable SELF;
    
    // storage slot 0 for length
    // slot keccak256(0) for data if length is > 31 bytes
    string public name;
    
    constructor () {
        SELF = address(this );
    }
    
    function setName (string calldata name_) external {
        require(msg.sender == ADMIN, "Not and admin");
        name = name:
    }
}    
```

## Solidity Functions

### What are solidity functions?

- Solidity functions are like open endpoints of smart contracts. Anyone can call and use them. 
It is fully a smart contract’s responsibility to add proper access control checks and guarantees that the execution is safe for the end user.

```solidity
contract Example {
    function foo(uint256 var) external view returns (address user);
}    
```

- `signature`: "foo(uint256)" (name of the function and type it accepts).
- `selector`: keccak256("foo(uint256)")[:4] = 0x2fbrbd38 (first 4 bytes of keccak256 hash).
- function selector is a special value that solidity uses to distinguish which function you are calling from the outside, to enter the exact function you're calling.

### Functions and variables visibility

- Functions can be `external`, `public`, `internal`, and `private`.
- Variables can be `public`, `internal`, `private`, and `default`.

```solidity
contract Example {
    uint256 public counter;
    function increment() external returns (uint256) {
        return ++counter;
    }
}    
```

### Functions modifiers
- Functions can have built-in and custom modifiers applied. Built-in modifiers are: `view`, `pure`, `payable`, `override`, and `virtual`.

```solidity
contract Example {
    address public owner;
    modifier onlyOwner() {
        require(owner == msg.sender, "Not an owner");
        _;
    }
    constructor () {
        owner = msg. sender;
    }    
    function changeOwner (address newOwner) external onlyOwner {
        owner = newOwner;
    }
}    
```

### Creating contracts

- **Opcode create**. Address is calculated as `keccak256(RLP([<sender>, <nonce>]))[12:]`. Smart contracts also have nonces, but they start from 1.

- **Opcode create2**. Address is calculated as `keccak256(0xff, senderAddress, salt, keccak256(init_code))[12:]`

```solidity
contract Example {
    function deploy() external {
        A a = new A() ;
        B b = new B{salt: bytes32(1)}();
    }
}   
```

### Calling other contracts

```solidity
interface Callee {
    function foo(uint256 amount) external;
}
    
contract Example {
    function testCall(address callee) external { 
        // high-level call
        Callee(callee). foo(1);
    
        // low-level call
        (bool ok,) callee.call(abi.encodewithSignature("foo(uint256)", 1));
        
        // delegatecall
        (ok,) callee.delegatecall(abt.encodeWithSelector(Callee.foo.selector, 1));
        
        // staticcall
        (ok,) = callee.staticcall(abt.encodewithSelector(Callee.foo.selector, 1));
        
        // callcode DEPRECATED
        (ok,) callee.callcode(abi.encodewithSelector(Callee.foo,selector, 111;
    }
}
```

![Calls](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1688954575/Screenshot_from_2023-07-10_07-32-39_rnvzhd.png "The user calls contract A which then calls contract B. The table is built from B context.")

### Difference between this.bar() and bar()

```solidity
contract Example {
    function bar() public;
    function foo() external {
        this.bar(); // creates a subcontext = call
        bar(); // jump
    }
}    
```

- `this.bar()` creates a subcontext call, msg.sender inside `bar()` function will be address of example contract
- `bar()` msg.sender will be externally owned account

### ABI encoding brief

- Solidity distinguishes static and dynamic types. Static types are encoded in-place (head) and dynamic types are encoded with a calldata offset (head + tail).

```solidity
abi.encodeWithSignature("deposit(address,uint256)", msg. sender, le18);
-> 

0x47e7ef24
0000000000000000000000005b38da6a701c568545dcfcb03fcb875f56beddc4
0000000000000000000000000000000000000000000000000de0b6b3a7640000
```
- static types: (`uint256`, `uint128`, `bytes32`, encoded in place -> directly translated into some bytes).
- dynamic types: dynamic array, struct with dynamic arrays, `bytes`, `strings`. encoding of head & tail.

### What are events?

- Solidity events are an abstraction on top of the EVM’s logging functionality. Applications can subscribe and listen to these events through the RPC interface of an Ethereum client.

```solidity
contract Example {
    event ReceivedEther(uint256 amount);
    
    receive() external payable {
        emit ReceivedEther(msg.value);
    }
}    
```

### Indexed parameters & anonymous events
- Events have special “topics” that applications can filter events by.
- 3 topics maximum for normal and 4 for anonymous events.
- 0st topic for non-anonymous event is an event signature.

```solidity
contract Example {
    event ReceivedEther(uint256 indexed amount);
    event ReceivedEtherAnon(uint256 amount) anonymous;
    
    receive() external payable {
        emit ReceivedEther(msg.value);
        emit ReceivedEtherAnon(msg.value);
    }
}    
```

### Error handling in Solidity

- There are 3 types of error handling functions: require, revert, assert. Throw is deprecated. Assert used to consume all the provided gas, but Solidity 0.8.0 changed that.

```solidity
contract Example {
    error NotWhitelisted(address token);
    function deposit(address token, uint256 amount) external {
        assert (amount > 0);
        
        if (!whitelisted[token]) {
            revert Notwhitelisted(token) ;
        }
        
        require(IERC20(token).transferfrom(msg.sender, address(this), amount), "Transfer failed");
    }
}    
```

### Try/catch in Solidity
```solidity
contract Example { 
    error CustomError();
    
    function doRevert() external { 
        revert("Oops");
    }    

    function foo() external {
        try this.doRevert() {
            ...
        } catch Error(string memory reason) { // "Oops" will be here
            ...
        } catch Panic(uint256 code) {
            ...
        } catch (bytes memory reason) { // CustomError will be here
            ...
        } catch {// equivalent to catch (bytes memory reason)`
        
        }
    }
}    
```

### Checks-effects-interactions pattern
```solidity
contract Example { 
    mapping(address => uint256) public deposits;
    
    function deposit() external payable { 
        deposits [msg.sender] += msg.value;
    }

    function withdraw() external {
        (bool ok, ) = msg.sender.call({value: deposits[msg.sender]}("");
        require(ok, "Transfer failed");
        delete deposits [msg.sender];
    }
}    
```

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
