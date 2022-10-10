 # Low Risk Issues

## Summary Of Findings:

|  | Issue 
-- | -- 
1 | Use a more recent version of solidity

## Details on Findings:

### 1. <ins>Use a more recent version of solidity</ins>

All files under scope uses solidity version `0.7.6` which is 22 months (nearly 2 years) old.
```solidity
pragma solidity ^0.7.6;
```

There are clear advantages of using a recent solidity version. At least update it to the next breaking change version which is `0.8.0`. 

Remember to:
1. Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath
2. Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining
3. Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
4. Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
5. Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
6. Use a solidity version of at least 0.8.12 to get `string.concat()` to be used instead of `abi.encodePacked(<str>,<str>)`.

 # Non-Critical Issues

## Summary Of Findings:

|  | Issue 
-- | -- 
1 | Expressions for constant values such as a call to keccak256(), should use immutable rather than constant
2 | Non-library/interface files should use fixed compiler versions, not floating ones

## Details on Findings:

### 1. <ins>Expressions for constant values such as a call to keccak256(), should use immutable rather than constant</ins>

While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand. There is a difference between `constant` variables and `immutable` variables, and they should each be used in their appropriate contexts. `constants` should be used for literal values written into the code, and `immutable` variables should be used for expressions, or values calculated in, or passed into the constructor.

There are 4 instances of this in 1 file:

File: [GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L34-L45)

```solidity
    bytes32 private constant DOMAIN_TYPE_HASH =
    keccak256(
        "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract,bytes32 salt)"
        );
    bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
    bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
    bytes32 private constant PERMIT_TYPEHASH =
        keccak256(
            "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
        );
```

### 2. <ins>Non-library/interface files should use fixed compiler versions, not floating ones</ins>

All files under scope uses a floating pragma:
```solidity
pragma solidity ^0.7.6;
```

> Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

Source : [SWC-103](https://swcregistry.io/docs/SWC-103)
