1 - Check zero address implementation before the "acceptUpgrade" function.
==

It would worth to check the zero address implementation before the [acceptUpgrade](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L143) function, so the ```acceptUpgrade``` function would already have a non-zero implementation.

The zero address check could be in the [updgradeTo](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L115) function and that function could be used in the [constructor](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L57), so the ```constructor``` also can check the non-zero implementation. 


**Links:**

- [contracts/upgrades/GraphProxy.sol#L143](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L143)
- [contracts/upgrades/GraphProxy.sol#L116](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L116)
- [contracts/upgrades/GraphProxy.sol#L57](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L57)
