- [Low](#low)
    - [**1. Mixing and Outdated compiler**](#1-mixing-and-outdated-compiler)
    - [**2. Lack of checks address0**](#2-lack-of-checks-address0)
    - [**3. Lack of checks supportsInterface**](#3-lack-of-checks-supportsinterface)
- [Non critical](#non-critical)
    - [**4. Use abstract for base contracts**](#4-use-abstract-for-base-contracts)
    - [**5. Wrong comment**](#5-wrong-comment)
    - [**6. Use package with known vulenerabilities**](#6-use-package-with-known-vulenerabilities)
    - [**7. It's possible to call initialize twice**](#7-its-possible-to-call-initialize-twice)

# Low

## **1. Mixing and Outdated compiler**

The pragma version used are:

```
pragma solidity >=0.6.12 <0.8.0;
pragma solidity ^0.7.6;
```

*Note that mixing pragma is not recommended. Because different compiler versions have different meanings and behaviors, it also significantly raises maintenance costs. As a result, depending on the compiler version selected for any given file, deployed contracts may have security issues.*

The minimum required version must be [0.8.17](https://github.com/ethereum/solidity/releases/tag/v0.8.17); otherwise, contracts will be affected by the following **important bug fixes**:

[0.8.3](https://blog.soliditylang.org/2021/03/23/solidity-0.8.3-release-announcement/):

- Optimizer: Fix bug on incorrect caching of Keccak-256 hashes.

[0.8.4](https://blog.soliditylang.org/2021/04/21/solidity-0.8.4-release-announcement/):

- ABI Decoder V2: For two-dimensional arrays and specially crafted data in memory, the result of abi.decode can depend on data elsewhere in memory. Calldata decoding is not affected.

[0.8.9](https://blog.soliditylang.org/2021/09/29/solidity-0.8.9-release-announcement/):

- Immutables: Properly perform sign extension on signed immutables.
- User Defined Value Type: Fix storage layout of user defined value types for underlying types shorter than 32 bytes.

[0.8.13](https://blog.soliditylang.org/2022/03/16/solidity-0.8.13-release-announcement/):
- Code Generator: Correctly encode literals used in `abi.encodeCall` in place of fixed bytes arguments.

[0.8.14](https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/):

- ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against `calldatasize()` in all cases.
- Override Checker: Allow changing data location for parameters only when overriding external functions.

[0.8.15](https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/)

- Code Generation: Avoid writing dirty bytes to storage when copying `bytes` arrays.
- Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

[0.8.16](https://blog.soliditylang.org/2022/08/08/solidity-0.8.16-release-announcement/)

- Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

[0.8.17](https://blog.soliditylang.org/2022/09/08/solidity-0.8.17-release-announcement/)

- Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.

Apart from these, there are several minor bug fixes and improvements.

## **2. Lack of checks `address(0)`**

The following methods have a lack of checks if the received argument is an address, it's good practice in order to reduce human error to check that the address specified in the constructor or initialize is different than `address(0)`.

**Affected source code:**

- [Pausable.sol:55](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L55)
- [Governed.sol:31](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L31)

## **3. Lack of checks `supportsInterface`**

The EIP-165 standard helps detect that a smart contract implements the expected logic, prevents human error when configuring smart contract bindings, so it is recommended to check that the received argument is a contract and supports the expected interface.

**Reference:**

- https://eips.ethereum.org/EIPS/eip-165

**Affected source code:**

- [Managed.sol:105](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L105)
- [L2GraphToken.sol:71](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L71)
- [L2GraphToken.sol:61](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L61)
- [L2GraphTokenGateway.sol:99](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L99)
- [L2GraphTokenGateway.sol:109](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L109)
- [L2GraphTokenGateway.sol:119](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L119)
- [L1GraphTokenGateway.sol:112-113](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L112-L113)
- [L1GraphTokenGateway.sol:123](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L123)
- [L1GraphTokenGateway.sol:133](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L133)

---

# Non critical

## **4. Use `abstract` for base contracts**

`abstract` contracts are contracts that have at least one function without its implementation. **An instance of an abstract cannot be created.**

**Reference:**

- https://docs.soliditylang.org/en/v0.6.2/contracts.html#abstract-contracts

**Affected source code:**

- [GraphProxyStorage.sol:11](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L11)
- [GraphUpgradeable.sol:11](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L11)
- [Pausable.sol:5](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L5)
- [Managed.sol:23](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L23)
- [Governed.sol:9](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L9)

## **5. Wrong comment**

The contract `GraphProxy` has the following comment:

`NOTE: Only the admin can call this function.`

But this is not true, because the following methods use the modifier `ifAdminOrPendingImpl` or `ifAdmin`, and these modifiers will invoke `_fallback` if is not an admin.

**Affected source code:**

- [GraphProxy.sol:63](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L63)
- [GraphProxy.sol:76](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L76)
- [GraphProxy.sol:89](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L89)
- [GraphProxy.sol:102](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L102)
- [GraphProxy.sol:113](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L113)

## **6. Use package with known vulenerabilities**

Although not vulnerable, the version of openZeppelin used contains known vulnerabilities.

**Reference:**

- https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories/GHSA-4h98-2769-gh6h

**Affected source code:**

- [GraphTokenUpgradeable.sol:93](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L93)

## **7. It's possible to call `initialize` twice**

The method `initialize` should be allowed to be called only once.

**Affected source code:**

- [L1GraphTokenGateway.sol:99](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L99)
- [L2GraphToken.sol:48](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L48)
- [L2GraphTokenGateway.sol:87](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L87)
- [BridgeEscrow.sol:20](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L20)
