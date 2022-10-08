## 1. Use `x < y + 1` in stead of `x <= y`

_contracts/gateway/L1GraphTokenGateway.sol:_ [L224](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L224)
[L275](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L275)

_contracts/l2/token/GraphTokenUpgradeable.sol:_ [L95](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L95)

## 2. Cache storage variables in function call stack to save gas

_contracts/governance/Governed.sol:_ [L55-L66](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L55-L66)

_contracts/governance/Pausable.sol:_ [L27-L34](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L27-L34)
[L41-L48](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L41-L48)

## 3. You can make state variables take less slots than they currently are

_contracts/governance/Pausable.sol:_ [L8](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L8)

## 4. Use 1 and 2 for true and false

_contracts/governance/Pausable.sol:_ [L8](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L8)
[L10](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L10)

## 5. When comparing variables of type `uint`, use `require(x != 0)` instead of `require(x > 0)`

_contracts/gateway/L1GraphTokenGateway.sol:_ [L201](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L201)
[L217](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L217)

_contracts/l2/gateway/L2GraphTokenGateway.sol:_ [L146](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L146)

## 6. Calldata is cheaper than memory for function input

_contracts/arbitrum/L1ArbitrumMessenger.sol:_ [L48](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol#L48)
[L49](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol#L49)
[L75](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol#L75)

_contracts/arbitrum/L2ArbitrumMessenger.sol:_ [L41](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L2ArbitrumMessenger.sol#L41)

_contracts/l2/gateway/L2GraphTokenGateway.sol:_ [L266](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L266)
[L286](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L286)

_contracts/gateway/L1GraphTokenGateway.sol:_ [L290](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L290)
[L331](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L331)

_contracts/governance/Managed.sol:_ [L173](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L173)

## 7. Use multiple `require`s instead of a single one with `&&`

_contracts/governance/Governed.sol:_ [L54-L57](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L54-L57)

_contracts/gateway/L1GraphTokenGateway.sol:_ [L142](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142)

_contracts/upgrades/GraphProxy.sol:_ [L142-L145](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L142-L145)

## 8. Function that are called only once can be inlined in the calling function

### This change will save around 30 gas units

_contracts/l2/token/GraphTokenUpgradeable.sol:_ [L195](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L195)

_contracts/gateway/L1GraphTokenGateway.sol:_ [L290](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L290)

_contracts/l2/gateway/L2GraphTokenGateway.sol:_ [L286](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L286)

## 9. Use `constant` and `immutable` for constants

_contracts/governance/Managed.sol:_ [L29](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L29)

## 10. Use `immutable` instead of `constant` for the following to save runtime gas

_contracts/l2/token/GraphTokenUpgradeable.sol:_ [L38](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L38)
[L39](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L39)

## 11. Use `require(x)` instead of `require(x == true)`

_contracts/gateway/L1GraphTokenGateway.sol:_ [L214](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L214)
