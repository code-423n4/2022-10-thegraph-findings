

## Roundabout way of calculating L2GraphToken address
In L2GraphTokenGateway at [L156](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L156) and [L236](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L236) the L2GraphToken address is calculated as `calculateL2TokenAddress(l1GRT)`, but this always just returns `address(graphToken())`. Consider skipping the check performed in `calculateL2TokenAddress` by replacing `calculateL2TokenAddress(l1GRT)` with `address(graphToken())` at L156 and L236.

## No need for SafeMath for user protection
SafeMath is not needed [here](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L223) as it is only a check to protect the user against his own invalid inputs.

## Comparing to constant bool
Replace e.g. `foo == false` with `!foo`.

[`callhookWhitelist[msg.sender] == true`](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L214)

## Unused function
Consider removing the unused function [`Managed.graphTokenGateway()`](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L153-L155).

## Unnecessary address(0)-check
[`_pendingImplementation != address(0) && msg.sender == _pendingImplementation`](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L143) can be simplified to `msg.sender == _pendingImplementation` as `msg.sender` cannot be `address(0)`.

[`pendingGovernor != address(0) && msg.sender == pendingGovernor`](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L55) can be simplified to `msg.sender == pendingGovernor` as `msg.sender` cannot `address(0)`.

[`_escrow != address(0) && Address.isContract(_escrow)`](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142) can be simplified to `Address.isContract(_escrow)` as `address(0)` cannot be a contract.

## Don't allow invalid parameters with a unique valid value
`outboundTransfer()` and `finalizeInboundTransfer()`, in both L1GraphTokenGateway.sol and L2GraphTokenGateway.sol, have as first parameter `_l1Token` which is immediately checked for its only acceptable value. Instead of checking whether its value is correct, leave the parameter void and simply declare its value inside the function.

## Cache variable in memory
Move [Governed.sol#L60](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L60) to before L54 so you can use the cached `oldPendingGovernor` instead of `pendingGovernor` at L55. (Then also emit `oldPendingGovernor` instead of `governor` at L65.)

## Avoid accessing a storage variable when a cheaper alternative is available
Replace `pendingGovernor` with `_newGovernor` at [Governed.sol#L46](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L46).

Replace `pendingGovernor` with `address(0)` at [Governed.sol#L66](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L66).

Replace `_partialPaused` with `_toPause` at [Pausable.sol#L31](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L31) and [L34](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L34).

Replace `_paused` with `_toPause` at [Pausable.sol#L45](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L45) and [L48](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L48).

Replace `pauseGuardian` with `newPauseGuardian` at [Pausable.sol#L58](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L58)

Replace `gateway` with `_gw` at [L2GraphToken.sol#L62](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L62)


## `<` is cheaper than `<=`
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L95
