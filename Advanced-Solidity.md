## Advanced Solidity: Assembly and data locations

## What are opcodes?
- Opcodes are EVM instructions that the smart contract’s bytecode consists of. Opcodes can push/pop elements to/from the stack, access memory and storage, read calldata, and manipulate returndata. Opcodes are also used to call and create other contracts, and emit events.
- Examples: add (0x01), caller (0x33), mload (0x51), sstore (0x55), push32 (0x7f), delegatecall (0xf4), create2 (0xf5).

## What is Yul?
- Yul is a low-level programming language that can be used to write smart contract on its own or as an “inline assembly” inside Solidity. Yul is directly translated to opcodes and has to used with extreme caution. 
- Also the new Solidity codegen (IR compilation) uses Yul as an intermediate representation which is then translated to bytecode. This enables more optimization to take place.

## Solidity Inline Assembly

```solidity
contract Example {
    function sum(uint256 a, uint256 b) external pure returns(uint256) {
        uint256 res;
        assembly {
            res: = add(a,b);
        }
        
        return res;
    }
}
```

## Smart Contract in Yul

```solidity
object "Example" {
    code {
        datacopy(0, dataoffset("runtime"), datasize("runtime"))
        return(0, datasize("runtime"))
    }
    object "runtime" {
        code {
            switch selector()
            case 0xcad0899b /* sum(uint256, uint256) */
                let a:= calldataload(0x4) //load the first variable in calldata
                let b:= calldataload(0x24) //load the second variable in calldata
                
                mstore(0, add(a,b))
                return(0, 0x20)
        } default {revert(0,0}
        
        function selector() -> s {
            s:= shr(224, calldataload(0))
        }
    }
}
```        

## How does stack work?
- Opcodes manipulate stack. Some opcodes pop elements from it, some push. There are opcodes for arithmetical operations as well.
- Solidity compiler “remembers” where the value of local variables is stored on stack. 
- The opcodes are laid in the reverse polish notation (postfix).
- The stack size can’t exceed 1024 elements, otherwise the EVM will revert the transaction.

## How does memory work?
- Memory is a mapping between pointers and slots.
- The maximal value of a pointer is 2**256-1.
- Slots hold 32 byte values. There is no elements packing.
- There is a memory expansion cost (gas) the EVM tracks to limit the actual memory usage. The cost is quadratic to the maximal pointer used (read or write).
- Memory is reset for every new subcontext call. However, you can’t be sure that there is no garbage in the freshly allocated memory.

![Memory Space](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1691226280/Screenshot_from_2023-08-05_14-33-17_vmndn5.png)

## How does storage work?

- Storage is a mapping between slot numbers and slot values that are bound to a specific 
contract.
- The maximal slot number is 2**256-1.
- Slots hold 32 byte values. Elements might be packed.
- Storage is persistent and preserved between transactions.
- Depending on the current slot value, the value that we write right now, and the value before the transaction the cost is estimated.
- The length is stored at slot 0. The values are stored starting from slot keccak256(0).

```solidity
contract Example {
    uint256[] public arr;
    
    function foo() external {
        arr[0] = 1;
        arr[1] = 2;
    }
}
```
![Storage](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1691226760/Screenshot_from_2023-08-05_14-42-03_va3ywi.png)
- The length is stored at slot 0. The values are stored starting from slot keccak256(0). 

## How does calldata work?

- Calldata is a sequential data location very similar to memory.
- Calldata is read only for smart contracts.
- For every zero calldata byte 4 gas is paid. For every non-zero 16 gas.
- When smart contract is called, new calldata subcontext is allocated.
- Usually calldata is ABI encoded. Also it may be a smart contract bytecode in case of deployment transaction.

```solidity
contract Example {
    function foo(bytes calldata someBytes) external pure returns (bytes calldata) {
        return someBytes;
    }
}
```
-Calldata can be returned from the function. In that case it is just “forwarded” to the returndata.

## How does returndata work?

- Returndata is a sequential data location very similar to memory.
- Returndata can be written by return and revert opcodes.
- Returndata always stores the value of the latest called smart contract.
- Usually returndata is ABI encoded, regardless of the opcode used.
