# QA REPORT

## [LOW] In the following functions consider verifying the fee parameter
Where the fee parameter validation is checking greater than 0% (which may happen by mistake) and less than 100%

Example: [Staking.sol#L406](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/Staking.sol#L406)

## [LOW] Approve 0 first
At some tokens you can approve an amount (at USDT for instance) only after approving to 0. Consider using increase/decrease approve notation instead.

### Proof of concept:
- [AllocationExchange.sol#L77](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/statechannels/AllocationExchange.sol#L77)
- [GRTWithdrawHelper.sol#L65](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/statechannels/GRTWithdrawHelper.sol#L65)

## [LOW] Missing pause functionality


### Proof of concept:
- [Governed.sol](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Governed.sol)
- [AllocationExchange.sol](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/statechannels/AllocationExchange.sol)

## [LOW] Not verified input
At the following functions you should verify the parameters that are being assigned to a state variable.

### Proof of concept:
- [L1GraphTokenGateway.sol#L121](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L121)
- [L2GraphToken.sol#L69](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L69)

## [LOW] The project is compiled with different solidity versions


## [LOW] Payable functions that should not be payable
The following functions are payable but doesn't record the sender transaction. Consider making them not payable instead.

### Proof of concept:
- [L2GraphTokenGateway.sol#L232](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L232)
- [L1GraphTokenGateway.sol#L269](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L269)

## [LOW] Missing nonReentrancy modifier
The following functions allows attackers to try reentrancy since they are calling to external contracts / transferring eth. Consider adding a nonReentrancy modifier.

### Proof of concept:
- [L1GraphTokenGateway.sol#L152](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L152)
- [Multicall.sol#L17](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/base/Multicall.sol#L17)

## [LOW] Consider replacing assert with require
Assertions are a bad practice, use require instead.

### Proof of concept:
- [GraphProxy.sol#L50](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L50)
- [GraphProxy.sol#L47](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L47)

## [LOW] Use safeApprove
Use safeApprove in the following locations

### Proof of concept:
- [AllocationExchange.sol#L77](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/statechannels/AllocationExchange.sol#L77)
- [BridgeEscrow.sol#L28](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/BridgeEscrow.sol#L28)

## [NON CRITICAL] NonReentrancy should be the first modifier in order


Example: [L2GraphTokenGateway.sol#L232](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L232)

## [NON CRITICAL] Floating pragma
Floating pragma is a bad practice, since it does not guaranty the same version at future deployments.

### Proof of concept:
- [Controller.sol](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Controller.sol)
- [L2ArbitrumMessenger.sol](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/arbitrum/L2ArbitrumMessenger.sol)

## [NON CRITICAL] Consider emitting an event at the following functions


### Proof of concept:
- [GraphToken.sol#L64](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/GraphToken.sol#L64)
- [GraphTokenUpgradeable.sol#L150](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L150)

## [NON CRITICAL] Missing function spec comments


### Proof of concept:
- [GNS.sol#L855](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/discovery/GNS.sol#L855)
- [SubgraphNFT.sol#L107](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/discovery/SubgraphNFT.sol#L107)

## [NON CRITICAL] Unused function parameters should have name removed
If for any reason the following unused parameters are necessary then remove their naming (since only the type matters for function signature)

Example: [L2ArbitrumMessenger.sol#L42](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/arbitrum/L2ArbitrumMessenger.sol#L42)

## [NON CRITICAL] The following events are not indexed


### Proof of concept:
- [Pausable.sol#L20](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Pausable.sol#L20)
- [L2GraphToken.sol#L30](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L30)

