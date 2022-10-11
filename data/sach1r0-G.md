# Functions that are not called within the contract must set its visibility to `external` instead of `public`

## Details
Setting function's visibility to external when it is only called externally can save gas because external function’s parameters are not copied into memory and are instead read from calldata directly.
see reference: https://github.com/code-423n4/2021-06-gro-findings/issues/37

## Mitigation
Set function visibility to `external`

## Line of code
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L30
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L43
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L55
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L68
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L77

___
# `!=` is cheaper in gas compared to `>` for uint

## Details
`!= 0` costs less gas compared to `> 0` for unsigned integers in require statements with the optimizer enabled (6 gas)
see reference: https://github.com/code-423n4/2021-12-maple-findings/issues/75

## Mitigation
use `!= 0` instead of `> 0`

## Line of code
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L146

___
# Breaking down multiple `require` statements instead of using `&&`

## Details
Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.
See reference: [G-09] of https://code4rena.com/reports/2022-04-backd/

## Mitigation
I suggest breaking down two conditions into two `require` statement instead of using `&&`. Example:
Changing from:
```
require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```
to:
```
require(_escrow != address(0), "INVALID_ESCROW");
require(Address.isContract(_escrow), "INVALID_ESCROW");
```

## Line of code
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142
