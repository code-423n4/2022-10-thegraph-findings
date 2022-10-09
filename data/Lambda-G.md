# Unnecessary address(0) checks
In two places (https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/governance/Governed.sol#L55 & https://github.com/code-423n4/2022-10-thegraph/blob/7ea88cc41f17f2d49961aafec7ebe72daeaad3f9/contracts/upgrades/GraphProxy.sol#L143), the comparison with `address(0)` could be removed because the address is afterwards compared to `msg.sender`, which of course cannot be zero (or only with negligible probability to be precise).
# _fallback does not need free memory pointer
In `_fallback`, one can directly write to `0x0` instead of loading the free memory pointer. After the execution of the delegatecall, the execution is over anyways (either with a revert or a assembly return), so it does not matter if the memory is dirty. OpenZeppelin does the same in their implementation (https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/proxy/Proxy.sol#L27):
```
// Copy msg.data. We take full control of memory in this inline assembly
// block because it will not return to Solidity code. We overwrite the
// Solidity scratch pad at memory position 0.
```
Because this code block is executed very often (on every call), the savings would be quite significant.