# The Graph L2 Bridge Contest Gas Optimization Report

## Summary

The following gas optimization issues were found during the code review:

1. Use `calldata` instead of `memory` (3 instances)
2. Long `require()`/`revert()` string (4 instances)
3. Using `bool`s for storage incurs overhead (2 instances)
4. Use `!= 0` instead of `> 0` when comparing uint (4 instances)
5. Split `require(xxx && yyy)` to `require(xxx)` and `require(yyy)` (1 instance)
6. Use `abi.encodePacked()` instead of `abi.encode()` (6 instances)
7. Use custom errors instead of `revert()`/`require()` strings (55 instances)
 
Total 75 instances of 7 issues.

## 1. Use `calldata` instead of `memory` (3 instances)

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for loop to copy each index of the `calldata` to the `memory` index. This overhead can be optimized by using `calldata` directly.

```solidity
contracts/governance/Managed.sol::173 => function _syncContract(string memory _name) internal {

contracts/l2/gateway/L2GraphTokenGateway.sol::286 => function parseOutboundData(bytes memory _data) private view returns (address, bytes memory) {

contracts/gateway/L1GraphTokenGateway.sol::290 => function parseOutboundData(bytes memory _data)
```

## 2. Long `require()`/`revert()` string (4 instances)

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

```solidity
contracts/upgrades/GraphUpgradeable.sol::32 => require(msg.sender == _implementation(), "Caller must be the implementation");

contracts/upgrades/GraphProxy.sol::105 => require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

contracts/upgrades/GraphProxy.sol::141 => require(Address.isContract(_pendingImplementation), "Implementation must be a contract");

contracts/governance/Managed.sol::53 => require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```

## 3. Using `bool`s for storage incurs overhead (2 instances)

Use `uint256(1)` and `uint256(2)` for true/false. Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

```solidity
contracts/governance/Pausable.sol::8 => bool internal _partialPaused;

contracts/governance/Pausable.sol::10 => bool internal _paused;
```

## 4. Use `!= 0` instead of `> 0` when comparing uint (4 instances)

When dealing with unsigned integer types, comparisons with `!= 0` are cheaper then with `> 0`.

```solidity
contracts/l2/gateway/L2GraphTokenGateway.sol::146 => require(_amount > 0, "INVALID_ZERO_AMOUNT");

contracts/l2/gateway/L2GraphTokenGateway.sol::238 => if (_data.length > 0) {

contracts/gateway/L1GraphTokenGateway.sol::201 => require(_amount > 0, "INVALID_ZERO_AMOUNT");

contracts/gateway/L1GraphTokenGateway.sol::217 => require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```

## 5. Split `require(xxx && yyy)` to `require(xxx)` and `require(yyy)` (1 instance)

Instead of using operator && on single require check, using double `require()` checks can save more gas.

```solidity
contracts/gateway/L1GraphTokenGateway.sol::142 => require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```

## 6. Use `abi.encodePacked()` instead of `abi.encode()` (6 instances)

`abi.encodePacked()` is more efficient than `abi.encode()`.

```solidity
contracts/l2/token/GraphTokenUpgradeable.sol::88 => abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)

contracts/l2/token/GraphTokenUpgradeable.sol::162 => abi.encode(

contracts/l2/gateway/L2GraphTokenGateway.sol::174 => return abi.encode(id);

contracts/l2/gateway/L2GraphTokenGateway.sol::275 => abi.encode(0, _data) // we don't need to track exitNums (b/c we have no fast exits) so we always use 0

contracts/gateway/L1GraphTokenGateway.sol::249 => return abi.encode(seqNum);

contracts/gateway/L1GraphTokenGateway.sol::342 => abi.encode(emptyBytes, _data)
```

## 7. Use custom errors instead of `revert()`/`require()` strings (55 instances)

Using `require()`/`revert()` strings is expensive. Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors.

Custom errors decrease both deploy and runtime gas costs. Note that runtime gas cost is only relevant when the revert condition is met.

```solidity
contracts/upgrades/GraphUpgradeable.sol::24 => require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");

contracts/upgrades/GraphUpgradeable.sol::32 => require(msg.sender == _implementation(), "Caller must be the implementation");

contracts/governance/Governed.sol::24 => require(msg.sender == governor, "Only Governor can call");

contracts/governance/Governed.sol::41 => require(_newGovernor != address(0), "Governor must be set");

contracts/l2/token/L2GraphToken.sol::36 => require(msg.sender == gateway, "NOT_GATEWAY");

contracts/l2/token/L2GraphToken.sol::49 => require(_owner != address(0), "Owner must be set");

contracts/l2/token/L2GraphToken.sol::60 => require(_gw != address(0), "INVALID_GATEWAY");

contracts/l2/token/L2GraphToken.sol::70 => require(_addr != address(0), "INVALID_L1_ADDRESS");

contracts/upgrades/GraphProxyStorage.sol::62 => require(msg.sender == _admin(), "Caller must be admin");

contracts/upgrades/GraphProxy.sol::105 => require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

contracts/upgrades/GraphProxy.sol::141 => require(Address.isContract(_pendingImplementation), "Implementation must be a contract");

contracts/upgrades/GraphProxy.sol::157 => require(msg.sender != _admin(), "Cannot fallback to proxy target");

contracts/governance/Managed.sol::44 => require(!controller.paused(), "Paused");

contracts/governance/Managed.sol::45 => require(!controller.partialPaused(), "Partial-paused");

contracts/governance/Managed.sol::49 => require(!controller.paused(), "Paused");

contracts/governance/Managed.sol::53 => require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");

contracts/governance/Managed.sol::57 => require(msg.sender == address(controller), "Caller must be Controller");

contracts/governance/Managed.sol::104 => require(_controller != address(0), "Controller must be set");

contracts/l2/token/GraphTokenUpgradeable.sol::60 => require(isMinter(msg.sender), "Only minter can call");

contracts/l2/token/GraphTokenUpgradeable.sol::94 => require(_owner == recoveredAddress, "GRT: invalid permit");

contracts/l2/token/GraphTokenUpgradeable.sol::95 => require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");

contracts/l2/token/GraphTokenUpgradeable.sol::106 => require(_account != address(0), "INVALID_MINTER");

contracts/l2/token/GraphTokenUpgradeable.sol::115 => require(_minters[_account], "NOT_A_MINTER");

contracts/l2/token/GraphTokenUpgradeable.sol::123 => require(_minters[msg.sender], "NOT_A_MINTER");

contracts/l2/gateway/L2GraphTokenGateway.sol::98 => require(_l2Router != address(0), "INVALID_L2_ROUTER");

contracts/l2/gateway/L2GraphTokenGateway.sol::108 => require(_l1GRT != address(0), "INVALID_L1_GRT");

contracts/l2/gateway/L2GraphTokenGateway.sol::118 => require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");

contracts/l2/gateway/L2GraphTokenGateway.sol::145 => require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

contracts/l2/gateway/L2GraphTokenGateway.sol::146 => require(_amount > 0, "INVALID_ZERO_AMOUNT");

contracts/l2/gateway/L2GraphTokenGateway.sol::147 => require(msg.value == 0, "INVALID_NONZERO_VALUE");

contracts/l2/gateway/L2GraphTokenGateway.sol::148 => require(_to != address(0), "INVALID_DESTINATION");

contracts/l2/gateway/L2GraphTokenGateway.sol::153 => require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");

contracts/l2/gateway/L2GraphTokenGateway.sol::233 => require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

contracts/l2/gateway/L2GraphTokenGateway.sol::234 => require(msg.value == 0, "INVALID_NONZERO_VALUE");

contracts/gateway/L1GraphTokenGateway.sol::74 => require(inbox != address(0), "INBOX_NOT_SET");

contracts/gateway/L1GraphTokenGateway.sol::78 => require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");

contracts/gateway/L1GraphTokenGateway.sol::82 => require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");

contracts/gateway/L1GraphTokenGateway.sol::110 => require(_inbox != address(0), "INVALID_INBOX");

contracts/gateway/L1GraphTokenGateway.sol::111 => require(_l1Router != address(0), "INVALID_L1_ROUTER");

contracts/gateway/L1GraphTokenGateway.sol::122 => require(_l2GRT != address(0), "INVALID_L2_GRT");

contracts/gateway/L1GraphTokenGateway.sol::132 => require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");

contracts/gateway/L1GraphTokenGateway.sol::142 => require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

contracts/gateway/L1GraphTokenGateway.sol::153 => require(_newWhitelisted != address(0), "INVALID_ADDRESS");

contracts/gateway/L1GraphTokenGateway.sol::154 => require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");

contracts/gateway/L1GraphTokenGateway.sol::165 => require(_notWhitelisted != address(0), "INVALID_ADDRESS");

contracts/gateway/L1GraphTokenGateway.sol::166 => require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");

contracts/gateway/L1GraphTokenGateway.sol::200 => require(_l1Token == address(token), "TOKEN_NOT_GRT");

contracts/gateway/L1GraphTokenGateway.sol::201 => require(_amount > 0, "INVALID_ZERO_AMOUNT");

contracts/gateway/L1GraphTokenGateway.sol::202 => require(_to != address(0), "INVALID_DESTINATION");

contracts/gateway/L1GraphTokenGateway.sol::217 => require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

contracts/gateway/L1GraphTokenGateway.sol::224 => require(msg.value >= expectedEth, "WRONG_ETH_VALUE");

contracts/gateway/L1GraphTokenGateway.sol::271 => require(_l1Token == address(token), "TOKEN_NOT_GRT");

contracts/gateway/L1GraphTokenGateway.sol::275 => require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");

contracts/gateway/GraphTokenGateway.sol::31 => require(_newPauseGuardian != address(0), "PauseGuardian must be set");

contracts/gateway/GraphTokenGateway.sol::40 => require(!_paused, "Paused (contract)");
```
