1. 0 address checks should be done

One should always check for 0 addresses in case the contract is initialized by zero address by mistake. Like in the following line

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L32

When the governor is first set, it could happen that `_initGovernor` is `address(0)` which would lead to inability to change governor.

Mitigation: 
Add the following line: `require(_initGovernor) != address(0)`