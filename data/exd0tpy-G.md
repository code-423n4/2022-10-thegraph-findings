### Use != 0 instead of > 0 for Unsigned Integer Comparison

```
L1GraphTokenGateway.sol::201 => require(_amount > 0, "INVALID_ZERO_AMOUNT");
L1GraphTokenGateway.sol::217 => require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
L2GraphTokenGateway.sol::146 => require(_amount > 0, "INVALID_ZERO_AMOUNT");
L2GraphTokenGateway.sol::238 => if (_data.length > 0) {
```

