## [G-01] Use != 0 instead >0 for unsigned integer
## code snipped
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L146
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L201
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L217
## recommendation
for unsigned integer, use !=0 is more cheaper gas fee then >0, so we recommend to use !=0 instead of >0.

## [G-02] Missing revert message in require statement
## code snipped
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L34
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L48
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L59
## recommendation
if we write a require statement, we need condition and revert message when in other condition.  these require statement missing the revert message 
so we recommend to add revert message to know when the condition unreachable.

## [G-03] Use require instead &&
## code snipped
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L55
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L142
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142
## recommendation
When using logical conjunction (&&), make sure to order your functions correctly for optimal gas usage. we reccomend to use require instead of logical conjunction (&&)  for save gas cost. 

