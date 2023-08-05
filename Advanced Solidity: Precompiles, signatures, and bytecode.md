## Advanced Solidity: Precompiles, signatures, and bytecode

### What are precompiles?

- Precompiles are special smart contracts that come natively with Ethereum. They are built on top of EVM to extend its functionality. Currently there are 9 precompiled contracts available. All of them live on addresses from (0x01) to (0x09).

### What precompiles are available?

- ecrecover (0x01), ECDSA recovery algorithm.
- sha256 (0x02), sha256 hash function implementation.
- ripemd160 (0x03), ripemd160 hash function implementation.
- identity (0x04), returns whatever is input. Used to copy memory.
- modexp (0x05), calculates (B^E) % M with arbitrary precision.
- ecadd (0x06), alt_bn128 elliptic curve addition.
- ecmul (0x07), alt_bn128 elliptic curve multiplication.
- ecpairing (0x08), alt_bn128 elliptic curve pairing.
- blake2f (0x09), compression function used in blake2 hash function.

### Precompiles properties

- Precompiled contract never revert. They either return 0 or nothing if an error occurs.
- EXCODESIZE and similar opcodes treat precompiles as EOAs. Meaning that these opcodes do not detect any bytecode under precompiles.
- Precompiles work with calldata. Even though the accepted format differs from ABI encoding, it is still possible to call precompiles this way.

### Vault unlock challenge
- The unlock function passes if address(0x02) or address(0x03) are supplied.

### Digital signatures in Ethereum

- Ethereum users mainly utilize 2 types of digital signatures:
ECDSA in the execution client to sign transactions and messages. Also precompile (0x01) is used to recover the signer on-chain.
- [EIP-191](https://eips.ethereum.org/EIPS/eip-191) and [EIP-712](https://eips.ethereum.org/EIPS/eip-712) describe how to construct and sign messages.
- BLS in the consensus client to sign slots attestations. BLS is used as those kind of signatures might be easily aggregated for network optimization.

### Why use message signatures?
- There are several use cases where message signatures may be used:
- May be used by trusted parties to “approve” some sort of action for users.
- May be used in a multisig.
- May be used to let users sign platform terms and conditions.
- May be used as an entropy to an encryption key generator.

### EIP-191 example
- Replay attacks are possible: cross-chain, cross-contract, cross-function.

```solidity
```

### EIP-712 example
- Replay attacks are mitigated.
```solidity
```

### EIP-191 vs EIP-712

- Always consider using EIP-712 over EIP-191.
- Never use EIP-191 if it is a message that users have to sign.
- EIP-191 is needed to distinguish between message & transaction signatures.
- Proper EIP-712 makes message signature replay attacks impossible.
- EIP-712 is a bit more expensive and cumbersome to work with.

### Smart contracts bytecode
- Init code is the contract’s bytecode after its compilation including ABI encoded constructor arguments. This is the same bytecode that is used in the tx “data” field. Init code can’t be more than 48kb.
- Runtime code is the contract’s bytecode after its constructor execution. Runtime code can’t be more than 24kb.

### Compilation example
```solidity
```

### Example bytecode breakdown
```solidity
```
