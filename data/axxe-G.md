## Split `require`

In [Governed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L55) : 

```solidity
function acceptOwnership() external {
    require(
        pendingGovernor != address(0) && msg.sender == pendingGovernor,
        "Caller must be pending governor"
    );
    (...)
}
```

We should split the require : 

```solidity
function acceptOwnership() external {
    require(pendingGovernor != address(0), "No pending governor"); 
    require(msg.sender == pendingGovernor, "Caller must be pending governor");
    (...)
}
```