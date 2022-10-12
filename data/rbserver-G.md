## [G-01] `msg.sender` CAN BE USED INSTEAD OF STATE VARIABLE IF POSSIBLE
Reading `msg.sender` costs 2 gas, which is much cheaper than reading a state variable because an `sload` operation costs 100 or 2100 gas depending on whether the accessed address is warm or not.

In the following `acceptOwnership` function, `msg.sender` can be used instead of `pendingGovernor` for setting `oldPendingGovernor` and updating `governor`. `msg.sender` can also be used instead of `governor` for emitting the `NewOwnership` event.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L53-L67
```solidity
    function acceptOwnership() external {
        require(
            pendingGovernor != address(0) && msg.sender == pendingGovernor,
            "Caller must be pending governor"
        );

        address oldGovernor = governor;
        address oldPendingGovernor = pendingGovernor;

        governor = pendingGovernor;
        pendingGovernor = address(0);

        emit NewOwnership(oldGovernor, governor);
        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
    }
```

## [G-02] `address(0)` CAN BE USED INSTEAD OF STATE VARIABLE IF POSSIBLE
Executing `address(0)` instead of reading a state variable saves gas by avoiding the costly `sload` operation.

In the following `acceptOwnership` function, `address(0)` can be used instead of `pendingGovernor` for emitting the `NewPendingOwnership` event.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L53-L67
```solidity
    function acceptOwnership() external {
        require(
            pendingGovernor != address(0) && msg.sender == pendingGovernor,
            "Caller must be pending governor"
        );

        address oldGovernor = governor;
        address oldPendingGovernor = pendingGovernor;

        governor = pendingGovernor;
        pendingGovernor = address(0);

        emit NewOwnership(oldGovernor, governor);
        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
    }
```

## [G-03] MEMORY VARIABLES CAN BE USED INSTEAD OF STATE VARIABLES IF POSSIBLE
Accessing a memory variable instead of a state variable replaces an `sload` operation with an `mload` operation. When this is possible, gas is saved.

In the following `transferOwnership` function, `_newGovernor` can be used instead of `pendingGovernor` for emitting the `NewPendingOwnership` event.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L40-L47
```solidity
    function transferOwnership(address _newGovernor) external onlyGovernor {
        require(_newGovernor != address(0), "Governor must be set");

        address oldPendingGovernor = pendingGovernor;
        pendingGovernor = _newGovernor;

        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
    }
```

In the following `setGateway` function, `_gw` can be used instead of `gateway` for emitting the `GatewaySet` event.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L59-L63
```solidity
    function setGateway(address _gw) external onlyGovernor {
        require(_gw != address(0), "INVALID_GATEWAY");
        gateway = _gw;
        emit GatewaySet(gateway);
    }
```

In the following `_setPaused` function, `_toPause` can be used instead of `_paused` for emitting the `PauseChanged` event.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L40-L49
```solidity
    function _setPaused(bool _toPause) internal {
        if (_toPause == _paused) {
            return;
        }
        _paused = _toPause;
        if (_paused) {
            lastPauseTime = block.timestamp;
        }
        emit PauseChanged(_paused);
    }
```

In the following `_setPauseGuardian` function, `newPauseGuardian` can be used instead of `pauseGuardian` for emitting the `NewPauseGuardian` event.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L55-L59
```solidity
    function _setPauseGuardian(address newPauseGuardian) internal {
        address oldPauseGuardian = pauseGuardian;
        pauseGuardian = newPauseGuardian;
        emit NewPauseGuardian(oldPauseGuardian, pauseGuardian);
    }
```

## [G-04] STATE VARIABLE THAT IS ACCESSED FOR MULTIPLE TIMES CAN BE CACHED IN MEMORY
A state variable that is accessed for multiple times can be cached in memory, and the cached variable value can then be used. This can replace multiple `sload` operations with one `sload`, one `mstore`, and multiple `mload` operations to save gas.

In the following `permit` function, `nonces[_owner]` can be cached, and the cached value can then be used for setting `digest` and updating `nonces[_owner]`.
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L74-L99
```solidity
    function permit(
        ...
    ) external {
        bytes32 digest = keccak256(
            abi.encodePacked(
                "\x19\x01",
                DOMAIN_SEPARATOR,
                keccak256(
                    abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
                )
            )
        );

        ...

        nonces[_owner] = nonces[_owner].add(1);
        ...
    }
```

## [G-05] BOOLEAN EXPRESSION CAN BE USED INSTEAD OF COMPARING BOOLEAN VALUE TO BOOLEAN LITERAL
Comparing a boolean value to a boolean literal needs the `iszero` operation and costs more gas than using a boolean expression.

`callhookWhitelist[msg.sender]` can be used instead of `callhookWhitelist[msg.sender] == true` in the following code.

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L214
```solidity
      extraData.length == 0 || callhookWhitelist[msg.sender] == true,
```

## [G-06] `require` REASON STRINGS CAN BE SHORTENED TO FIT IN 32 BYTES
`require` reason strings that are longer than 32 bytes need more memory space and more `mstore` operations than these that are shorter than 32 bytes when reverting. Please consider shortening the following `require` reason strings.

```solidity
contracts\gateway\GraphTokenGateway.sol
  21: "Only Governor or Guardian can call"

contracts\governance\Managed.sol
  53: require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");

contracts\upgrades\GraphUpgradeable.sol
  32: require(msg.sender == _implementation(), "Caller must be the implementation");
  
contracts\upgrades\GraphProxy.sol
  141: require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
  144: "Caller must be the pending implementation"
```

## [G-07] NEWER VERSION OF SOLIDITY CAN BE USED
The protocol can benefit from more gas-efficient features, such as custom errors starting from Solidity v0.8.4, by using a newer version of Solidity. For the following contracts, please consider using a newer Solidity version.

```solidity
contracts\gateway\GraphTokenGateway.sol
  3: pragma solidity ^0.7.6;

contracts\gateway\L1GraphTokenGateway.sol
  3: pragma solidity ^0.7.6;

contracts\governance\Governed.sol
  3: pragma solidity ^0.7.6;

contracts\governance\Managed.sol
  3: pragma solidity ^0.7.6;

contracts\governance\Pausable.sol
  3: pragma solidity ^0.7.6;

contracts\l2\gateway\L2GraphTokenGateway.sol
  3: pragma solidity ^0.7.6;

contracts\l2\token\GraphTokenUpgradeable.sol
  3: pragma solidity ^0.7.6;

contracts\l2\token\L2GraphToken.sol
  3: pragma solidity ^0.7.6;

contracts\upgrades\GraphProxy.sol
  3: pragma solidity ^0.7.6;

contracts\upgrades\GraphProxyAdmin.sol
  3: pragma solidity ^0.7.6;

contracts\upgrades\GraphProxyStorage.sol
  3: pragma solidity ^0.7.6;

contracts\upgrades\GraphUpgradeable.sol
  3: pragma solidity ^0.7.6;
```