# L-01: Setting implementation uses a 2 step procedure but setting of admin does not.

This is counterintuitive as the change of admin is of a higher importance than change of implementation. In the case of a user error, where a wrong address is set when changing implementation, it can always be fixed by calling the function again. However, change of admin has the `ifAdmin` modifier, and changing it to a wrong address is significantly more dangerous.

[GraphProxyStorage.sol#L130](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L130)
```solidity
    function _setPendingImplementation(address _newImplementation) internal {
        address oldPendingImplementation = _pendingImplementation();

        bytes32 slot = PENDING_IMPLEMENTATION_SLOT;
        assembly {
            sstore(slot, _newImplementation)
        }

        emit PendingImplementationUpdated(oldPendingImplementation, _newImplementation);
    }
}
```

**Mitigation**:

In my opinion, use of 2 step procedure is optional for change of implementation, whereas it should be used for change of admin as it an important role.