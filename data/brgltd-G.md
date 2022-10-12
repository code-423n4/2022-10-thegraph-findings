# [01] Use != 0 instead of > 0 to save gas.

For unsigned integer types, comparisons with `!= 0` are cheaper then with `> 0`.

There are 3 instances of this issue.

```
File: contracts/gateway/L1GraphTokenGateway.sol
201: require(_amount > 0, "INVALID_ZERO_AMOUNT");
217: require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```

```
File: contracts/l2/gateway/L2GraphTokenGateway.sol
146: require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

# [02] Update to solidity >= 0.8.4 and use custom errors rather than require/revert strings to save gas

Custom errors are available from solidity version 0.8.4 and can save ~50 gas each time they’re hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas.

There are 56 instances of this issue.

```
File: contracts/gateway/GraphTokenGateway.sol
31: require(_newPauseGuardian != address(0), "PauseGuardian must be set");
40: require(!_paused, "Paused (contract)");
```

```
File: contracts/gateway/L1GraphTokenGateway.sol
74: require(inbox != address(0), "INBOX_NOT_SET");
78: require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");
82: require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");
110: require(_inbox != address(0), "INVALID_INBOX");
111: require(_l1Router != address(0), "INVALID_L1_ROUTER");
122: require(_l2GRT != address(0), "INVALID_L2_GRT");
132: require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");
142: require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
153: require(_newWhitelisted != address(0), "INVALID_ADDRESS");
154: require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
165: require(_notWhitelisted != address(0), "INVALID_ADDRESS");
166: require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
200: require(_l1Token == address(token), "TOKEN_NOT_GRT");
201: require(_amount > 0, "INVALID_ZERO_AMOUNT");
202: require(_to != address(0), "INVALID_DESTINATION");
217: require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
224: require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
271: require(_l1Token == address(token), "TOKEN_NOT_GRT");
275: require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
```

```
File: contracts/governance/Governed.sol
24: require(msg.sender == governor, "Only Governor can call");
41: require(_newGovernor != address(0), "Governor must be set");
```

```
File: contracts/governance/Managed.sol
44: require(!controller.paused(), "Paused");
45: require(!controller.partialPaused(), "Partial-paused");
49: require(!controller.paused(), "Paused");
53: require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
57: require(msg.sender == address(controller), "Caller must be Controller");
104: require(_controller != address(0), "Controller must be set");
```

```
File: contracts/l2/gateway/L2GraphTokenGateway.sol
98: require(_l2Router != address(0), "INVALID_L2_ROUTER");
108: require(_l1GRT != address(0), "INVALID_L1_GRT");
118: require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
145: require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
146: require(_amount > 0, "INVALID_ZERO_AMOUNT");
147: require(msg.value == 0, "INVALID_NONZERO_VALUE");
148: require(_to != address(0), "INVALID_DESTINATION");
153: require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
233: require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
234: require(msg.value == 0, "INVALID_NONZERO_VALUE");
```

```
File: contracts/l2/token/GraphTokenUpgradeable.sol
60: require(isMinter(msg.sender), "Only minter can call");
94: require(_owner == recoveredAddress, "GRT: invalid permit");
95: require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
106: require(_account != address(0), "INVALID_MINTER");
115: require(_minters[_account], "NOT_A_MINTER");
123: require(_minters[msg.sender], "NOT_A_MINTER");
```

```
File: contracts/l2/token/L2GraphToken.sol
36: require(msg.sender == gateway, "NOT_GATEWAY");
49: require(_owner != address(0), "Owner must be set");
60: require(_gw != address(0), "INVALID_GATEWAY");
70: require(_addr != address(0), "INVALID_L1_ADDRESS");
```

```
File: contracts/upgrades/GraphProxy.sol
105: require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
141: require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
157: require(msg.sender != _admin(), "Cannot fallback to proxy target");
179: revert(ptr, size)
```

```
File: contracts/upgrades/GraphProxyStorage.sol
62: require(msg.sender == _admin(), "Caller must be admin");
```

```
File: contracts/upgrades/GraphUpgradeable.sol
24: require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
32: require(msg.sender == _implementation(), "Caller must be the implementation");
```

# [03] Don’t compare boolean expressions to boolean literals

if (<x> == true) => if (<x>), if (<x> == false) => if (!<x>)

There's 1 instance of this issue.

```
File: contracts/gateway/L1GraphTokenGateway.sol
214: extraData.length == 0 || callhookWhitelist[msg.sender] == true,
```

