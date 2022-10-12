[N-01] Use chainid instead of assembly code.

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L199

Use block.chainid instead of         
```
assembly {
            id := chainid()
        }
```