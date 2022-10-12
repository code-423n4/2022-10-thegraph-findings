## Use of Block.timestamp

`block.timestamp` is risky, as it can be manipulated by miners

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L32
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L46
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L95

## Use descriptive reasons for `require` statements

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L47
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L34

## Use `external` instead of `public` functions

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L55
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L68
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L43
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L30

## Use `indexed` fields in events

This makes the parameters more quickly accessible to off-chain tools that parse events.

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L19
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L20
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L24
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L26
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L30

## Don't use unlocked pragma

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L3 

## Update pragma to use latest version of solidity compiler like 0.8.15

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L3
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L3 

## Missing zero address validation

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L80-L87
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L104-L109
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L69-L74
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L93-L98
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L115-L124
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L130-L139
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L115-L117