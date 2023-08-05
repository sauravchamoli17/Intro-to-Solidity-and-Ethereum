## Proxy and libraries

## What are proxies?

- Proxies are special lightweight smart contracts that are placed on top of regular contracts (implementations) and are needed to provide upgradeability and deployment expenses savings. Proxy contracts work courtesy of delegatecall opcode.

![Proxies](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1691228130/Screenshot_from_2023-08-05_15-05-16_a8roxt.png)

### How do proxies work?

- Proxies are standalone contracts that leverage the concept of a fallback function and a delegatecall.
- Proxies keep the implementation address in a special slot to mitigate storage collisions.
- When a user calls a proxy, the call is forwarded to a fallback function. The fallback then delegatecalls the implementation, using the provided calldata.
- The implementation bytecode is executed on the proxy storage.

### When fallback and receive are called?

- receive function must be payable, can be virtual.
- fallback function may be payable, can be virtual.

![Fallback&Recieve](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1691228290/Screenshot_from_2023-08-05_15-07-52_vobngp.png)

### What is a storage collision?

- Storage collision happens when a proxy and an implementation contracts use the same slot N to store their data. Due to the delegatecall, one contract might override the data of another.

![StorageCollision](https://res.cloudinary.com/dg6ijhjsn/image/upload/v1691228363/Screenshot_from_2023-08-05_15-09-09_zewahc.png)

### Proxy Example

```solidity
```

### What benefits do proxies provide?
- Proxy contracts can upgrade their implementations to overcome the bytecode immutability limitation.
- Proxy contracts are usually small (1 kb) which makes them less expensive to deploy.
- Proxy contracts enable the usage of amazing patterns like Diamond proxy and Contracts Dependencies Registry.

### What are the downsides of proxies?

- Because proxies may upgrade their implementations, using them reduces the trust to the protocols.
- Upgrading contracts is not simple due to storage collisions. Usually contract upgrades bring bugs.
- Clients might think that proxies are “a cure for all diseases”. However proxies should only be used for bug fixes & optimization purposes.

### Proxy standards

- Transparent proxy
- UUPS proxy
- Beacon proxy
- Minimal proxy (clones)
- Diamond proxy
- Inherited storage proxy

### What are libraries?

- Libraries are contracts that can’t have storage, constructors, fallback, receive functions and can’t be inherited from. Solidity has 2 types of libraries: internal and external. Internal are inlined and called via jumps while external are called via delegatecalls and have to be specifically linked to the callees.

### “Using” directive

- “Using” keyword is a solidity syntax sugar to indicate that certain variable has to be passed first to a library function.

```solidity
```

### What are internal libraries?

- Internal libraries are stateless contracts that can’t be deployed on their own. They can only have internal and private functions. Internal libraries are inlined into the “main” contract during compilation.

```solidity
```

### What are external libraries?

- External libraries are stateless standalone contracts that have external functions. External libraries are deployable and have to be linked with the “main” contract after compilation. Non-view external functions can only be delegatecalled.

```solidity
```

### How does external library linking work?

- During compilation Solidity “reserves” certain bytecode where external library is called. “Reservation” is the first 17 bytes of keccak256 hash of a library full name. 
- The linker looks for those reserved places and substitutes the placeholder with the actual library address.

### How do external libraries help?
- External libraries work with extended ABI encoding. They can work with storage references.
- External libraries are separate contracts, so they help with bytecode splitting.
- External libraries are stateless. They can be deployed once and used by many other contracts.

## External library real world example

```solidity
```
