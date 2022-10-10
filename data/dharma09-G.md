## 1.COMPARISONS: != IS MORE EFFICIENT THAN > IN REQUIRE (6 GAS LESS)
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas)

For uints the minimum value would be 0 and never a negative value. Since it cannot be a negative value, then the check > 0 is essentially checking that the value is not equal to 0 therefore >0 can be replaced with !=0 which saves gas.

While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas.

## PROOF OF CONCEPT

Instances include:

### L1GraphTokenGateway.sol
- [L1GraphTokenGateway.sol#L201](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L201)
- [L1GraphTokenGateway.sol#L217](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L217)

### L2GraphTokenGateway.sol
- [L2GraphTokenGateway.sol#L146](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L146)


## 2. Require statements including conditions with the `&&` operator can be broken down in multiple require statements to save gas.

## PROOF OF CONCEPT
Instances include:
- [L1GraphTokenGateway.sol#L142](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142)
```
  require(_escrow != address(0), "INVALID_ESCROW");
  require( Address.isContract(_escrow), "INVALID_ESCROW");
```
- [Governed.sol#L54](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L54)
```
 require( pendingGovernor != address(0),"Caller must be pending governor");
 require(msg.sender == pendingGovernor,"Caller must be pending governor");
```

- [GraphProxy.sol#L142](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L142)

```
require(_pendingImplementation != address(0)"Caller must be the pending implementation");
require(msg.sender == _pendingImplementation,"Caller must be the pending implementation");
```
