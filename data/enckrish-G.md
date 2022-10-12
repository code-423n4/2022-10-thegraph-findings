## [G-1] Use `_notPaused()` inside `Managed.notPartialPaused` to remove duplicated code and reduce gas usage
Reduces Staking deploy gas by 37000, but also increases GNS deploy gas by 36000. Should be applied based on the deploy preferences.

## [G-2] Set allowance to 0 instead of decreasing allowance by approved amount
BridgeEscrow.revokeAll sets approval of `_spender` to zero. The current implementation, i.e.
```sol
IGraphToken grt = graphToken();
grt.decreaseAllowance(_spender, grt.allowance(address(this), _spender));
```
can be changed to:
```sol
graphToken().approve(_spender, 0);
```
This decreases gas used per call by 1600 and per deployment by 43000.

## [G-3] Unnecessary zero-address check for msg.sender in Governed.acceptOwnership
The affected code:
```sol
require(
    pendingGovernor != address(0) && msg.sender == pendingGovernor,
    "Caller must be pending governor"
);
```
`pendingGovernor` is required to be `msg.sender`, and if that condition satisfies it cannot be the zero-address. So that check can be safely removed.