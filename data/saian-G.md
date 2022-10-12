
## Reduce size of revert stings

Revert strings that are longer than 32 bytes requires at least one additional mstore, along with additional overhead for computing memory offset, etc.
Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L32

```
    require(msg.sender == _implementation(), "Caller must be the implementation"); 

```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L53

```
    require(msg.sender == controller.getGovernor(), "Caller must be Controller governor"); 
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L105

```
    require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```

# Use function arguments or local variables instead of storage variables in event emits

In event emits using local variables or function arguments instead of storage variable can avoid storage read and save gas

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L46

```
    emit NewPendingOwnership(oldPendingGovernor, pendingGovernor); 

```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L65-L66

```
    emit NewOwnership(oldGovernor, governor); 
    emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
```

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L58

```
    emit NewPauseGuardian(oldPauseGuardian, pauseGuardian);
```

## require statement can be split into multiple statements

Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L55

```
    require(
        pendingGovernor != address(0) && msg.sender == pendingGovernor,
        "Caller must be pending governor"
    );
```

## Reduce storage reads to save gas

Storage values that are read multiple times in the same function can be cached to avoid expensive SLOAD and save gas

`pendingGovernor` can be replaced by variable `oldPendingGovernor`

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L62

```
    address oldPendingGovernor = pendingGovernor;

    governor = pendingGovernor;
```

`_partialPause` can be replaced by `_toPause`

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L31

```
    _partialPaused = _toPause;
    if (_partialPaused)
```

`_paused` can be replaced by `_toPause`

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L44

```
    _paused = _toPause;
    if (_paused)
```

# Switching from non-zero to non-zero is more efficient

By switching from non-zero to non-zero value, expensive SSTORE from 0/false to non-zero can be avoided. 
and when boolean is used, writing to storage costs more than uint256 as each write operation has to perform extra read operation to correctly place the boolean value in the slot.
So switching from 0/false to 1/true can be replaced by 1 and 2

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L8-L10

```
    bool internal _partialPaused;
   
    bool internal _paused;
```
