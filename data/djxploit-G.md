## Use `!=0` instead of `>0` inside a require statement

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L146
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L201
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L217 

## Multiple mappings can be combined into a single mapping to a struct

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L51-L52

##  Using `bool`s for storage incurs overhead 

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L8
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L10
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L35

##  Split `require` statements having multiple condition checks

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L55
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L143
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142

## Use `abi.encodePacked` instead of `abi.encode` to save gas

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L88
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L162
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L174