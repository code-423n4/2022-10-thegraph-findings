1. Instead of using && in a require, using double require can save gas
In the following line: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L55
the require statement requires two conditions to be met. Instead of having just one require, two save gas one could have two requires one for each condition.

Mitigation:
`require(pendingGovernor != address(0), "pending governor address 0")`
`require(msg.sender == pendingGovernor, "Caller must be pending governor");`