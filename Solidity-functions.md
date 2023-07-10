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
