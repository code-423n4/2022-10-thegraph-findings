## 1. use of floating pragma

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

- [GraphProxyStorage.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol)
- [GraphProxy.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol)
- [GraphUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol)
- [GraphProxyAdmin.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol)
- [Managed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol)
- [Governed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol)
- [Pausable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol)
- [GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol)
- [L2GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol)
- [L2GraphToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol)
- [L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol)
- [BridgeEscrow.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol)
- [GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol)


## 2. require()/revert() statements should have descriptive reason strings or custom error

- [GraphProxy.sol#L133](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L133)

- [GraphProxyAdmin.sol#L34](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L34)
- [GraphProxyAdmin.sol#L47](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L47)
- [GraphProxyAdmin.sol#L59](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L59)


## 3. Outdated compiler version

Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs


## 4. inconsistent use of named return variables

there is an inconsistent use of named return variables in the contract some functions return named variables, others return explicit values. consider adopting a consistent approach.
this would improve both the explicitness and readability of the code, and it may also help reduce regressions during future code refactors.

- [GraphProxyStorage.sol#L69](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L69)
- [GraphProxyStorage.sol#L93](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L93)
- [GraphProxyStorage.sol#L104](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L104)

- [GraphUpgradeable.sol#L40](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L40)


## 5. require() should be used instead of assert()

Prior to solidity version 0.8.0, hitting an assert consumes the remainder of the transaction’s available gas rather than returning it, as require()/revert() do. assert() should be avoided even past solidity version 0.8.0 as its documentation states that “The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix”.

- [GraphProxy.sol#L47](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L47)
- [GraphProxy.sol#L48](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L48)
- [GraphProxy.sol#L51](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L51)


## 6. typo in comments

tranfer --> transfer

- [L1GraphTokenGateway.sol#L185](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L185)

Recepient --> Recipient

- [L1GraphTokenGateway.sol#L260](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L260)


## 7. _safemint() should be used rather than _mint() wherever possible

- [L2GraphToken.sol#L81](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L81)

- [GraphTokenUpgradeable.sol#L133](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L133)
- [GraphTokenUpgradeable.sol#L155](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L155)
