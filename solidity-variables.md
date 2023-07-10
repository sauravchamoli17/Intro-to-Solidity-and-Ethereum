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
