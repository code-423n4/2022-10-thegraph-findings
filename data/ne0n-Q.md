1. 0 address checks should be done

One should always check for 0 addresses in case the contract is initialized by zero address by mistake. Like in the following line

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L32

When the governor is first set, it could happen that `_initGovernor` is `address(0)` which would lead to inability to change governor.

Note: While this is an internal function, the caller of the function also does not check for 0 address.
- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L22
- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L152

Mitigation: 
Add the following line: `require(_initGovernor) != address(0)`

2. Avoid assembly whenever required
`assembly{ id := chainid() }` => `uint256 id = block.chainid`

Instance: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L199

2. It is a good programming practice to use constants instead of magic numbers

- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L33
- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L46
- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L58

A constant variable could be defined for each hex value.