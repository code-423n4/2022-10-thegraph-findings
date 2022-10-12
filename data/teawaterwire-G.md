unnecessary call to `decreaseAllowance` in `BridgeEscrow.sol` 's `revokeAll` function,

since the allowance is set back to 0, the `approve` method can be used:

```solidity
    function revokeAll(address _spender) external onlyGovernor {
        graphToken().approve(_spender, 0);
        // IGraphToken grt = graphToken();
        // grt.decreaseAllowance(_spender, grt.allowance(address(this), _spender));
    }
```

running `yarn test:gas test/gateway/bridgeEscrow.test.ts` in both cases returns a gas optimization of 1596 units