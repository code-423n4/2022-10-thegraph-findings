| | issue |
| ----------- | ----------- |
| 1 | [state variables can be packed into fewer storage slots](#1-state-variables-can-be-packed-into-fewer-storage-slots) |
| 2 | [state variables should be cached in stack variables rather than re-reading them from storage](#2-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage) |
| 3 | [Stack variable used as a cheaper cache for a state variable is only used once](#3-stack-variable-used-as-a-cheaper-cache-for-a-state-variable-is-only-used-once) |
| 4 | [require()/revert() strings longer than 32 bytes cost extra gas](#4-requirerevert-strings-longer-than-32-bytes-cost-extra-gas) |
| 5 | [splitting require() statements that use `&&` saves gas](#5-splitting-require-statements-that-use--saves-gas) |
| 6 | [use a more recent version of solidity](#6-use-a-more-recent-version-of-solidity) |
| 7 | [using `bool` for storage incurs overhead](#7-using-bool-for-storage-incurs-overhead) |
| 8 | [internal functions only called once can be inlined to save gas](#8-internal-functions-only-called-once-can-be-inlined-to-save-gas) |
| 9 | [internal or private functions not called by the contract or inherited ones should be removed to save deployment gas](#9-internal-or-private-functions-not-called-by-the-contract-or-inherited-ones-should-be-removed-to-save-deployment-gas) |
| 10 | [bytes constants are more efficient than string constants](#10-bytes-constants-are-more-efficient-than-string-constants) |
| 11 | [public functions not called by the contract should be declared external instead](#11-public-functions-not-called-by-the-contract-should-be-declared-external-instead) |
| 12 | [Unused local variable](#12-unused-local-variable) |
| 13 | [expressions for constant values such as a call to keccak256(), should use immutable rather than constant](#13-expressions-for-constant-values-such-as-a-call-to-keccak256-should-use-immutable-rather-than-constant) |
| 14 | [using > 0 costs more gas than != 0 when used on a uint in a require() statement](#14-using--0-costs-more-gas-than--0-when-used-on-a-uint-in-a-require-statement) |
| 15 | [abi.encode() is less efficient than abi.encodepacked()](#15-abiencode-is-less-efficient-than-abiencodepacked) |
| 16 | [should use arguments instead of state variable](#16-should-use-arguments-instead-of-state-variable) |

## 1. state variables can be packed into fewer storage slots
If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables are also cheaper.

`pauseGuardian` should be right after or before the bools so they can be packed together

- [Pausable.sol#L8-17](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L8)


## 2. state variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching will replace each Gwarmaccess (100 gas) with a much cheaper stack read.

`pendingGovernor` should be cached after line 53

- [Governed.sol#L53](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L53)

`_partialPaused` should be cached after line 30

- [Pausable.sol#L30](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L30)

`_paused` should be cached after line 44

- [Pausable.sol#L44](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L44)


## 3. Stack variable used as a cheaper cache for a state variable is only used once
If the variable is only accessed once, it’s cheaper to use the state variable directly that one time

- [GraphProxyStorage.sol#L70](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L70)
- [GraphProxyStorage.sol#L81](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L81)
- [GraphProxyStorage.sol#L94](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L94)
- [GraphProxyStorage.sol#L105](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L105)
- [GraphProxyStorage.sol#L118](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L118)
- [GraphProxyStorage.sol#L133](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L133)

- [GraphUpgradeable.sol#L41](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L41)


## 4. require()/revert() strings longer than 32 bytes cost extra gas

- [Managed.sol#L53](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L53)

- [GraphProxy.sol#L105](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L105)
- [GraphProxy.sol#L141](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L141)
- [GraphProxy.sol#L142](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L142)

- [GraphUpgradeable.sol#L32](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L32)

- [GraphTokenGateway.sol#L19](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L19)


## 5. splitting require() statements that use `&&` saves gas

- [Governed.sol#L54](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L54)

- [GraphProxy.sol#L142](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L142)

- [L1GraphTokenGateway.sol#L142](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142)


## 6. use a more recent version of solidity
Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath
Use a solidity version of at least 0.8.2 to get compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have `external` calls skip contract existence checks if the external call has a return value	
Use a solidity version of at least 0.8.13 to get the ability to use `using for` with a list of free functions


## 7. using `bool` for storage incurs overhead

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

- [Pausable.sol#L8](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L8)
- [Pausable.sol#L10](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L10)
- [Pausable.sol#L26](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L26)
- [Pausable.sol#L40](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L40)

- [GraphProxy.sol#L132](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L132)

- [GraphProxyAdmin.sol#L33](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L33)
- [GraphProxyAdmin.sol#L46](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L46)
- [GraphProxyAdmin.sol#L58](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L58)

- [GraphTokenGateway.sol#L47](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L47)

- [L1GraphTokenGateway.sol#L35](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L35)

- [GraphTokenUpgradeable.sol#L51](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L51)


## 8. internal functions only called once can be inlined to save gas
Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

- [Managed.sol#L43](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L43)
- [Managed.sol#L48](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L48)
- [Managed.sol#L52](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L52)
- [Managed.sol#L56](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L56)

- [Pausable.sol#L26](https://github.com/code-423n4/2022-26-thegraph/blob/main/contracts/governance/Pausable.sol#L10)
- [Pausable.sol#L10](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L10)
- [Pausable.sol#L10](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L10)
- [Pausable.sol#L10](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L10)

- [GraphProxyStorage.sol#L115](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L115)

- [GraphTokenGateway.sol#L39](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L39)


## 9. internal or private functions not called by the contract or inherited ones should be removed to save deployment gas

- [Managed.sol#L153](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L153)


## 10. bytes constants are more efficient than string constants
If data can fit into 32 bytes, then you should use bytes32 datatype rather than bytes or strings as it is cheaper in solidity.

- [Managed.sol#L173](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L173)


## 11. public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents’ functions and change the visibility from external to public and can save gas by doing so. 

- [GraphProxyAdmin.sol#L68](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L68)
- [GraphProxyAdmin.sol#L77](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L77)


## 12. Unused local variable
Some of the variables are fetched and allocated but not used

`gatewayData` is not used so we dont need to even make it.
- [L2GraphTokenGateway.sol#L242](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L242)


## 13. expressions for constant values such as a call to keccak256(), should use immutable rather than constant

- [GraphTokenUpgradeable.sol#L34](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L34)
- [GraphTokenUpgradeable.sol#L38](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L38)
- [GraphTokenUpgradeable.sol#L39](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L39)
- [GraphTokenUpgradeable.sol#L40](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L40)
- [GraphTokenUpgradeable.sol#L42](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L42)


## 14. using > 0 costs more gas than != 0 when used on a uint in a require() statement

- [L1GraphTokenGateway.sol#L201](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L201)
- [L1GraphTokenGateway.sol#L217](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L217)

- [L2GraphTokenGateway.sol#L146](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L146)


## 15. abi.encode() is less efficient than abi.encodepacked()

- [L1GraphTokenGateway.sol#L249](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L249)
- [L1GraphTokenGateway.sol#L342](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L342)


## 16. should use arguments instead of state variable

- [L2GraphToken.sol#L62](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L62)

- [Governed.sol#L46](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L46)

- [Pausable.sol#L34](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L34)
- [Pausable.sol#L48](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L48)
- [Pausable.sol#L58](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L58)
