## OOP in Solidity

### What is OOP?

- Object-oriented programming (OOP) is a programming paradigm based on the concept of "objects", which can contain data and code. The data is in the form of attributes or properties, and the code is in the form of procedures or methods.

![OOP](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1691219047/hdipchsdvgyhq7ax4lfj.png)

- Classes: the declaration for the data format and available procedures for a given type or class of object. Classes contain the data members and member functions.

- Objects: instances of classes.

### SOLID Principles

- **The Single-responsibility principle**: there should never be more than one reason for a class to change. In other words, every class should have only one responsibility.

- **The Open-closed principle**: software entities should be open for extension, but closed for modification.

- **The Liskov substitution principle**: functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it.

- **The Interface segregation principle**: clients should not be forced to depend upon interfaces that they do not use.

- **The Dependency inversion principle**: depend upon abstractions, not concretions.

## Solidity and OOP

- There are no classes is Solidity but there are **contracts**.
- There are no objects as is. Every deployed **contract** can be thought of as an object.
- What contracts do is they define an ABI encoding/decoding interface to interact with each 
  other.
- There is no object <> class coupling. You can use any **contract** declaration to represent any deployed contract (as long as the interfaces match).

### Contract Declaration

```solidity
abstract contract A {
  function setA(uint256 a_) external virtual;
}
```

```solidity
contract A {
  uint256 public a;
  function setA(uint256 a_) external {
    a = a_;
  };
}
```

```solidity
interface IA {
  function setA(uint256 a_) external;
}
```

- Abstract contracts are contracts which can not be deployed but just have function declarations.
- Interface can't have: state variables, modifiers declared, extend from contracts, functions with bodies defined.
- Interfaces only define function signature & define what those functions return, virtual by default.(declare structs & events)

## Object <> class example

```solidity
import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.soL";
import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";

interface MyERC20 {
    function transfer(address to, utnt256 amount) external returns (bool);
    function balanceOf(address user) external view returns (uint256);
}

abstract contract MyOtherERC20 {
    function transfer(address, uint256) external virtual returns (bool);
    function balanceOf(address) external view virtual returns (utnt256);
}

contract A {
address public owner = msg.sender;
function transferToOwner(address token) external {
    // equivalent transfers
    ERC20(token).transfer(owner, ERC20(token).balanceOf(address(this)));
    IERC20(token).transfer(owner, IERC20( token). balanceOf(address(this)));
    MyERC20(token).transfer(owner, MyERC20( token). balanceOf(address(this)));
    MyOtherERC20(token).transfer(owner, MyOtherERC20( token). balanceOf(address(this)));
    }
}
```

## Solidity polymorphism

- Even though there are no traditional classes, Solidity supports contracts inheritance, function overloading and function overriding.

```solidity
contract A {
    function foo() external virtual returns (address) {
        return address(1);
    }
    
    function foo(uint256) external returns (address) {
        return address(1);
    }
}

contract B is A {
    function foo() external override returns (address) {
        return address(2);
    }
}
```

## Solidity multiple inheritance

- Solidity supports multiple inheritance and implements it the same way the Python does. It uses C3 linearization to force a specific order of classes in the directed acyclic graph (DAG) of base classes. In Solidity the order in “is” directive is important: it starts from the most-base and ends with the most-derived class (reversed to Python).

## Solidity Diamond Problem

- The Solidity diamond problem occurs when a smart contract inherits from multiple parent contracts, and some of these parents share a common ancestor with functions or state variables. This creates ambiguity, as Solidity cannot determine which parent's implementation to use. 

- To resolve this, Solidity employs the C3 Linearization algorithm to determine the order in which the parent contracts are linearized, ensuring each function or state variable is inherited only once. Developers must be mindful of this issue when using multiple inheritance in Solidity and carefully design their contract hierarchy to avoid conflicts and unexpected behavior.

![OOP](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1691221423/Screenshot_from_2023-08-05_13-12-26_aibeok.png)

## Solidity constructors execution order

- Legacy compilation:
Variables are evaluated in the top-to-bottom inheritance DAG order.
Constructors are executed in the top-to-bottom inheritance DAG order.

- Via IR (intermediate representation) compilation:
Variables together with constructors are executed in the top-to-bottom inheritance DAG order.

## Init code vs runtime code

- Init code is the contract’s bytecode after its compilation including ABI encoded constructor arguments. This is the same bytecode that is used in the tx “data” field. Init code can’t be more than 48kb.

- Runtime code is the contract’s bytecode after its constructor execution. Runtime code can’t be more than 24kb.

### Solidity storage inheritance
- Storage layout follows the inheritance DAG order.

```solidity
contract A {
    uint256 public a; //slot 0
}

contract B is A {
    uint256 public b; //slot 1
}

contract C is A {
    uint256 public c; //slot 1
}

contract D is C, B {
    // a -> slot 0
    // c -> slot 1
    // b -> slot 2
    uint256 public d; //slot 3
}
```
