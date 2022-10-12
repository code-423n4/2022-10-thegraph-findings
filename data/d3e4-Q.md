

## `calculateL2TokenAddress` returns `address(0)` for invalid inputs
In L2GraphTokenGateway.sol [`calculateL2TokenAddress`](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L203-L207) should "only work for GRT", i.e. we require `l1ERC20 == l1GRT`. But when this is not the case it simply returns `address(0)`. This would mean that the L2 address of a bridged token is `address(0)` which clearly cannot be the case.
Similarly for [`calculateL2TokenAddress` in L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L352-L358)
Consider reverting instead.

## Inconsistent use of  "owner"/"governor"
`address governor` is defined in [Governed.sol#L12](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L12). However, in several places throughout the code base this is referred to as "owner". In Governed.sol itself we have `NewPendingOwnership`, `NewOwnership`, `transferOwnership` and `acceptOwnership` mixed in with a preponderance of "governor". They should be renamed `NewPendingGovernorship`, `NewGovernorship`, `transferGovernorship` and `acceptGovernorship`, or similar. The same goes for all other uses of "owner" in the rest of the code base.

## Don't allow invalid parameters with a unique valid value
`outboundTransfer()` and `finalizeInboundTransfer()`, in both L1GraphTokenGateway.sol and L2GraphTokenGateway.sol, have as first parameter `_l1Token` which is immediately checked for its only acceptable value. Instead of checking whether its value is correct, leave the parameter void and simply declare its value inside the function.

## Floating pragmas
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

## Contracts don't inherit their interfaces

Instances:
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol

## Typos
["tranfer" -> "transfer"](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L185)
