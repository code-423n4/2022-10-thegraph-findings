# **Gas Optimization**

Serial No. | Issue Name | Instances
--- | --- | ---
G-1 | Using > 0 costs more gas than != 0 when used on a uint in a require() statement | 3
G-2 | Splitting require() statements that use && saves gas | 1
G-3 | Use custom errors rather than revert()/require() strings to save deployment gas | 55
G-4 | Usage of assert() instead of require() | 3
G-5 | Expressions for constant values such as a call to keccak256(), should use immutable rather than constant | 2
G-6 | abi.encode() is less efficient than abi.encodePacked() | 6
G-7 | Reduce the size of error messages (Long revert Strings) | 4
G-8 | Use calldata instead of memory in external functions | 2
G-9 | Using bools for storage incurs overhead | 4
G-10 | Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead | 8
G-11 | Use a more recent version of solidity | 23
G-12 | Use Assembly to check for address(0) | 23
G-13 | Inline a modifier that’s only used once | 5

----
***Total instances found in this contest: 139 | Among all files in scope***


## 1. Using > 0 costs more gas than != 0 when used on a uint in a require() statement

> 0 is less efficient than != 0 for unsigned integers (with proof)
!= 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas) Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas. You can see this tweet for more proof: https://twitter.com/gzeon/status/1485428085885640706

### Instances
[L2GraphTokenGateway.sol:L146](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L146)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:146:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
```
[L1GraphTokenGateway.sol:L201](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L201)
```
contracts/gateway/L1GraphTokenGateway.sol:201:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
```
[L1GraphTokenGateway.sol:L217](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L217)
```
contracts/gateway/L1GraphTokenGateway.sol:217:                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```

### Reference:

[https://twitter.com/gzeon/status/1485428085885640706](https://twitter.com/gzeon/status/1485428085885640706)

### Remediation:

I suggest changing > 0 with != 0. Also, please enable the Optimizer.

-----

## 2. Splitting require() statements that use && saves gas

Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.

### Instances
[L1GraphTokenGateway.sol:L142](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142)
```
contracts/gateway/L1GraphTokenGateway.sol:142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```

### Mitigation:
Breakdown each condition in a separate require statement (though require statements should be replaced with custom errors)

-----

## 3. Custom Errors instead of Revert Strings to save Gas

Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met). Starting from Solidity v0.8.4,there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");),but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.
Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

### Instances
[L2GraphTokenGateway.sol:L98](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L98)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:98:        require(_l2Router != address(0), "INVALID_L2_ROUTER");
```
[L2GraphTokenGateway.sol:L108](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L108)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:108:        require(_l1GRT != address(0), "INVALID_L1_GRT");
```
[L2GraphTokenGateway.sol:L118](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L118)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:118:        require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
```
[L2GraphTokenGateway.sol:L145](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L145)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:145:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
```
[L2GraphTokenGateway.sol:L146](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L146)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:146:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
```
[L2GraphTokenGateway.sol:L147](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L147)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:147:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
```
[L2GraphTokenGateway.sol:L148](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L148)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:148:        require(_to != address(0), "INVALID_DESTINATION");
```
[L2GraphTokenGateway.sol:L153](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L153)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:153:        require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
```
[L2GraphTokenGateway.sol:L233](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L233)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:233:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
```
[L2GraphTokenGateway.sol:L234](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L234)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:234:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
```
[L2GraphToken.sol:L36](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L36)
```
contracts/l2/token/L2GraphToken.sol:36:        require(msg.sender == gateway, "NOT_GATEWAY");
```
[L2GraphToken.sol:L49](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L49)
```
contracts/l2/token/L2GraphToken.sol:49:        require(_owner != address(0), "Owner must be set");
```
[L2GraphToken.sol:L60](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L60)
```
contracts/l2/token/L2GraphToken.sol:60:        require(_gw != address(0), "INVALID_GATEWAY");
```
[L2GraphToken.sol:L70](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L70)
```
contracts/l2/token/L2GraphToken.sol:70:        require(_addr != address(0), "INVALID_L1_ADDRESS");
```
[GraphTokenUpgradeable.sol:L60](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L60)
```
contracts/l2/token/GraphTokenUpgradeable.sol:60:        require(isMinter(msg.sender), "Only minter can call");
```
[GraphTokenUpgradeable.sol:L94](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L94)
```
contracts/l2/token/GraphTokenUpgradeable.sol:94:        require(_owner == recoveredAddress, "GRT: invalid permit");
```
[GraphTokenUpgradeable.sol:L95](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L95)
```
contracts/l2/token/GraphTokenUpgradeable.sol:95:        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```
[GraphTokenUpgradeable.sol:L106](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L106)
```
contracts/l2/token/GraphTokenUpgradeable.sol:106:        require(_account != address(0), "INVALID_MINTER");
```
[GraphTokenUpgradeable.sol:L115](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L115)
```
contracts/l2/token/GraphTokenUpgradeable.sol:115:        require(_minters[_account], "NOT_A_MINTER");
```
[GraphTokenUpgradeable.sol:L123](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L123)
```
contracts/l2/token/GraphTokenUpgradeable.sol:123:        require(_minters[msg.sender], "NOT_A_MINTER");
```
[GraphProxy.sol:L105](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L105)
```
contracts/upgrades/GraphProxy.sol:105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```
[GraphProxy.sol:L141](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L141)
```
contracts/upgrades/GraphProxy.sol:141:        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
```
[GraphProxy.sol:L157](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L157)
```
contracts/upgrades/GraphProxy.sol:157:        require(msg.sender != _admin(), "Cannot fallback to proxy target");
```
[GraphUpgradeable.sol:L24](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L24)
```
contracts/upgrades/GraphUpgradeable.sol:24:        require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
```
[GraphUpgradeable.sol:L32](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L32)
```
contracts/upgrades/GraphUpgradeable.sol:32:        require(msg.sender == _implementation(), "Caller must be the implementation");
```
[GraphProxyStorage.sol:L62](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L62)
```
contracts/upgrades/GraphProxyStorage.sol:62:        require(msg.sender == _admin(), "Caller must be admin");
```
[L1GraphTokenGateway.sol:L74](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L74)
```
contracts/gateway/L1GraphTokenGateway.sol:74:        require(inbox != address(0), "INBOX_NOT_SET");
```
[L1GraphTokenGateway.sol:L78](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L78)
```
contracts/gateway/L1GraphTokenGateway.sol:78:        require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");
```
[L1GraphTokenGateway.sol:L82](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L82)
```
contracts/gateway/L1GraphTokenGateway.sol:82:        require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");
```
[L1GraphTokenGateway.sol:L110](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L110)
```
contracts/gateway/L1GraphTokenGateway.sol:110:        require(_inbox != address(0), "INVALID_INBOX");
```
[L1GraphTokenGateway.sol:L111](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L111)
```
contracts/gateway/L1GraphTokenGateway.sol:111:        require(_l1Router != address(0), "INVALID_L1_ROUTER");
```
[L1GraphTokenGateway.sol:L122](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L122)
```
contracts/gateway/L1GraphTokenGateway.sol:122:        require(_l2GRT != address(0), "INVALID_L2_GRT");
```
[L1GraphTokenGateway.sol:L132](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L132)
```
contracts/gateway/L1GraphTokenGateway.sol:132:        require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");
```
[L1GraphTokenGateway.sol:L142](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142)
```
contracts/gateway/L1GraphTokenGateway.sol:142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```
[L1GraphTokenGateway.sol:L153](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L153)
```
contracts/gateway/L1GraphTokenGateway.sol:153:        require(_newWhitelisted != address(0), "INVALID_ADDRESS");
```
[L1GraphTokenGateway.sol:L154](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L154)
```
contracts/gateway/L1GraphTokenGateway.sol:154:        require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
```
[L1GraphTokenGateway.sol:L165](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L165)
```
contracts/gateway/L1GraphTokenGateway.sol:165:        require(_notWhitelisted != address(0), "INVALID_ADDRESS");
```
[L1GraphTokenGateway.sol:L166](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L166)
```
contracts/gateway/L1GraphTokenGateway.sol:166:        require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
```
[L1GraphTokenGateway.sol:L200](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L200)
```
contracts/gateway/L1GraphTokenGateway.sol:200:        require(_l1Token == address(token), "TOKEN_NOT_GRT");
```
[L1GraphTokenGateway.sol:L201](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L201)
```
contracts/gateway/L1GraphTokenGateway.sol:201:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
```
[L1GraphTokenGateway.sol:L202](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L202)
```
contracts/gateway/L1GraphTokenGateway.sol:202:        require(_to != address(0), "INVALID_DESTINATION");
```
[L1GraphTokenGateway.sol:L217](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L217)
```
contracts/gateway/L1GraphTokenGateway.sol:217:                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```
[L1GraphTokenGateway.sol:L224](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L224)
```
contracts/gateway/L1GraphTokenGateway.sol:224:                    require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
```
[L1GraphTokenGateway.sol:L271](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L271)
```
contracts/gateway/L1GraphTokenGateway.sol:271:        require(_l1Token == address(token), "TOKEN_NOT_GRT");
```
[L1GraphTokenGateway.sol:L275](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L275)
```
contracts/gateway/L1GraphTokenGateway.sol:275:        require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
```
[GraphTokenGateway.sol:L31](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L31)
```
contracts/gateway/GraphTokenGateway.sol:31:        require(_newPauseGuardian != address(0), "PauseGuardian must be set");
```
[GraphTokenGateway.sol:L40](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L40)
```
contracts/gateway/GraphTokenGateway.sol:40:        require(!_paused, "Paused (contract)");
```
[Managed.sol:L44](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L44)
```
contracts/governance/Managed.sol:44:        require(!controller.paused(), "Paused");
```
[Managed.sol:L45](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L45)
```
contracts/governance/Managed.sol:45:        require(!controller.partialPaused(), "Partial-paused");
```
[Managed.sol:L49](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L49)
```
contracts/governance/Managed.sol:49:        require(!controller.paused(), "Paused");
```
[Managed.sol:L53](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L53)
```
contracts/governance/Managed.sol:53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```
[Managed.sol:L57](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L57)
```
contracts/governance/Managed.sol:57:        require(msg.sender == address(controller), "Caller must be Controller");
```
[Managed.sol:L104](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L104)
```
contracts/governance/Managed.sol:104:        require(_controller != address(0), "Controller must be set");
```
[Governed.sol:L24](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L24)
```
contracts/governance/Governed.sol:24:        require(msg.sender == governor, "Only Governor can call");
```
[Governed.sol:L41](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L41)
```
contracts/governance/Governed.sol:41:        require(_newGovernor != address(0), "Governor must be set");
```

### Remediation:
I suggest replacing revert strings with custom errors.

-----

## 4. Usage of assert() instead of require()

Between solc 0.4.10 and 0.8.0, require() used REVERT (0xfd) opcode which refunded the remaining gas on failure while assert() used INVALID (0xfe) opcode which consumed all the supplied gas. (see [https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions](https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions)).
require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking (properly functioning code should never reach a failing assert statement, unless there is a bug in your contract you should fix).
From the current usage of assert, my guess is that they can be replaced with require unless a Panic really is intended.

### Instances:
[GraphProxy.sol:L47](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L47)
```
contracts/upgrades/GraphProxy.sol:47:        assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
```
[GraphProxy.sol:L48](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L48)
```
contracts/upgrades/GraphProxy.sol:48:        assert(
```
[GraphProxy.sol:L51](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L51)
```
contracts/upgrades/GraphProxy.sol:51:        assert(
```

### References:

[https://code4rena.com/reports/2022-05-bunker/#g-08-usage-of-assert-instead-of-require](https://code4rena.com/reports/2022-05-bunker/#g-08-usage-of-assert-instead-of-require)

-----

## 5. Expressions for constant values such as a call to keccak256(), should use immutable rather than constant

This results in the keccak operation being performed whenever the variable is used, increasing gas costs relative to just storing the output hash. Changing to immutable will only perform hashing on contract deployment which will save gas.

### Instances:
[GraphTokenUpgradeable.sol:L38](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L38)
```
contracts/l2/token/GraphTokenUpgradeable.sol:38:    bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
```
[GraphTokenUpgradeable.sol:L39](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L39)
```
contracts/l2/token/GraphTokenUpgradeable.sol:39:    bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
```
### References:

[https://github.com/code-423n4/2021-10-slingshot-findings/issues/3](https://github.com/code-423n4/2021-10-slingshot-findings/issues/3)


-----

## 6. abi.encode() is less efficient than abi.encodepacked()

Use abi.encodepacked() instead of abi.encode()

### Instances:
[L2GraphTokenGateway.sol:L174](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L174)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:174:        return abi.encode(id);
```
[L2GraphTokenGateway.sol:L275](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L275)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:275:                abi.encode(0, _data) // we don't need to track exitNums (b/c we have no fast exits) so we always use 0
```
[GraphTokenUpgradeable.sol:L88](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L88)
```
contracts/l2/token/GraphTokenUpgradeable.sol:88:                    abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
```
[GraphTokenUpgradeable.sol:L162](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L162)
```
contracts/l2/token/GraphTokenUpgradeable.sol:162:            abi.encode(
```
[L1GraphTokenGateway.sol:L249](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L249)
```
contracts/gateway/L1GraphTokenGateway.sol:249:        return abi.encode(seqNum);
```
[L1GraphTokenGateway.sol:L342](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L342)
```
contracts/gateway/L1GraphTokenGateway.sol:342:                abi.encode(emptyBytes, _data)
```
### Reference:

[https://code4rena.com/reports/2022-06-notional-coop#15-abiencode-is-less-efficient-than-abiencodepacked](https://code4rena.com/reports/2022-06-notional-coop#15-abiencode-is-less-efficient-than-abiencodepacked)


-----


## 7. Reduce the size of error messages (Long revert Strings)

Shortening revert strings to fit in 32 bytes will decrease deployment time gas and will decrease runtime gas when the revert condition is met.

Revert strings that are longer than 32 bytes require at least one additional mstore, along with additional overhead for computing memory offset, etc.

### Instances:
[GraphProxy.sol:L105](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L105)
```
contracts/upgrades/GraphProxy.sol:105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```
[GraphProxy.sol:L141](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L141)
```
contracts/upgrades/GraphProxy.sol:141:        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
```
[GraphUpgradeable.sol:L32](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L32)
```
contracts/upgrades/GraphUpgradeable.sol:32:        require(msg.sender == _implementation(), "Caller must be the implementation");
```
[Managed.sol:L53](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L53)
```
contracts/governance/Managed.sol:53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```
### Recommendations:

I suggest shortening the revert strings to fit in 32 bytes, or using custom errors as described next.

Custom errors from Solidity 0.8.4 are cheaper than revert strings
(cheaper deployment cost and runtime cost when the revert condition is
met)

Source [Custom Errors](https://blog.soliditylang.org/2021/04/21/custom-errors/) in Solidity:


-----

## 8. Use calldata instead of memory in external functions

Use calldata instead of memory in external functions to save gas.

### Instances:
[L2GraphTokenGateway.sol:L193](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L193)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:193:    ) external returns (bytes memory) {
```
[L1GraphTokenGateway.sol:L198](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L198)
```
contracts/gateway/L1GraphTokenGateway.sol:198:    ) external payable override notPaused returns (bytes memory) {
```
### Reference:

[https://code4rena.com/reports/2022-04-badger-citadel/#g-12-stakedcitadeldepositall-use-calldata-instead-of-memory](https://code4rena.com/reports/2022-04-badger-citadel/#g-12-stakedcitadeldepositall-use-calldata-instead-of-memory)


-----

## 9. Using bools for storage incurs overhead

```
// Booleans are more expensive than uint256 or any type that takes up a full
// word because each write operation emits an extra SLOAD to first read the
// slot's contents, replace the bits taken up by the boolean, and then write
// back. This is the compiler's defense against contract upgrades and
// pointer aliasing, and it cannot be disabled.
```

[Refer Here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27) Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past

### Instances:
[GraphTokenUpgradeable.sol:L51](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L51)
```
contracts/l2/token/GraphTokenUpgradeable.sol:51:    mapping(address => bool) private _minters;
```
[L1GraphTokenGateway.sol:L35](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L35)
```
contracts/gateway/L1GraphTokenGateway.sol:35:    mapping(address => bool) public callhookWhitelist;
```
[Pausable.sol:L8](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L8)
```
contracts/governance/Pausable.sol:8:    bool internal _partialPaused;
```
[Pausable.sol:L10](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L10)
```
contracts/governance/Pausable.sol:10:    bool internal _paused;
```
### References:

[https://code4rena.com/reports/2022-06-notional-coop#8-using-bools-for-storage-incurs-overhead](https://code4rena.com/reports/2022-06-notional-coop#8-using-bools-for-storage-incurs-overhead)


-----

## 10. Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)
Use a larger size then downcast where needed

### Instances:
[IStaking.sol:L47](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L47)
```
contracts/staking/IStaking.sol:47:        uint32 _indexingRewardCut,
```
[IStaking.sol:L48](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L48)
```
contracts/staking/IStaking.sol:48:        uint32 _queryFeeCut,
```
[IStaking.sol:L49](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L49)
```
contracts/staking/IStaking.sol:49:        uint32 _cooldownBlocks
```
[IStakingData.sol:L37](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStakingData.sol#L37)
```
contracts/staking/IStakingData.sol:37:        uint32 cooldownBlocks; // Blocks to wait before updating parameters
```
[IStakingData.sol:L38](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStakingData.sol#L38)
```
contracts/staking/IStakingData.sol:38:        uint32 indexingRewardCut; // in PPM
```
[IStakingData.sol:L39](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStakingData.sol#L39)
```
contracts/staking/IStakingData.sol:39:        uint32 queryFeeCut; // in PPM
```
[GraphTokenUpgradeable.sol:L79](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L79)
```
contracts/l2/token/GraphTokenUpgradeable.sol:79:        uint8 _v,
```
[IGraphToken.sol:L33](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L33)
```
contracts/token/IGraphToken.sol:33:        uint8 _v,
```
### References:

[https://code4rena.com/reports/2022-04-phuture#g-14-usage-of-uintsints-smaller-than-32-bytes-256-bits-incurs-overhead](https://code4rena.com/reports/2022-04-phuture#g-14-usage-of-uintsints-smaller-than-32-bytes-256-bits-incurs-overhead)


-----

## 11. Use a more recent version of solidity

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

### Instances:
[ICuration.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L3)
```
contracts/curation/ICuration.sol:3:pragma solidity ^0.7.6;
```
[IGraphCurationToken.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/IGraphCurationToken.sol#L3)
```
contracts/curation/IGraphCurationToken.sol:3:pragma solidity ^0.7.6;
```
[IStaking.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L3)
```
contracts/staking/IStaking.sol:3:pragma solidity >=0.6.12 <0.8.0;
```
[IStakingData.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStakingData.sol#L3)
```
contracts/staking/IStakingData.sol:3:pragma solidity >=0.6.12 <0.8.0;
```
[L2GraphTokenGateway.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L3)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:3:pragma solidity ^0.7.6;
```
[L2GraphToken.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L3)
```
contracts/l2/token/L2GraphToken.sol:3:pragma solidity ^0.7.6;
```
[GraphTokenUpgradeable.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L3)
```
contracts/l2/token/GraphTokenUpgradeable.sol:3:pragma solidity ^0.7.6;
```
[IGraphProxy.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L3)
```
contracts/upgrades/IGraphProxy.sol:3:pragma solidity ^0.7.6;
```
[GraphProxyAdmin.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L3)
```
contracts/upgrades/GraphProxyAdmin.sol:3:pragma solidity ^0.7.6;
```
[GraphProxy.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L3)
```
contracts/upgrades/GraphProxy.sol:3:pragma solidity ^0.7.6;
```
[GraphUpgradeable.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L3)
```
contracts/upgrades/GraphUpgradeable.sol:3:pragma solidity ^0.7.6;
```
[GraphProxyStorage.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L3)
```
contracts/upgrades/GraphProxyStorage.sol:3:pragma solidity ^0.7.6;
```
[L1GraphTokenGateway.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L3)
```
contracts/gateway/L1GraphTokenGateway.sol:3:pragma solidity ^0.7.6;
```
[ICallhookReceiver.sol:L9](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/ICallhookReceiver.sol#L9)
```
contracts/gateway/ICallhookReceiver.sol:9:pragma solidity ^0.7.6;
```
[BridgeEscrow.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L3)
```
contracts/gateway/BridgeEscrow.sol:3:pragma solidity ^0.7.6;
```
[GraphTokenGateway.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L3)
```
contracts/gateway/GraphTokenGateway.sol:3:pragma solidity ^0.7.6;
```
[IEpochManager.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L3)
```
contracts/epochs/IEpochManager.sol:3:pragma solidity ^0.7.6;
```
[IRewardsManager.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L3)
```
contracts/rewards/IRewardsManager.sol:3:pragma solidity ^0.7.6;
```
[Pausable.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L3)
```
contracts/governance/Pausable.sol:3:pragma solidity ^0.7.6;
```
[Managed.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L3)
```
contracts/governance/Managed.sol:3:pragma solidity ^0.7.6;
```
[Governed.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L3)
```
contracts/governance/Governed.sol:3:pragma solidity ^0.7.6;
```
[IController.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L3)
```
contracts/governance/IController.sol:3:pragma solidity >=0.6.12 <0.8.0;
```
[IGraphToken.sol:L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L3)
```
contracts/token/IGraphToken.sol:3:pragma solidity ^0.7.6;
```
### References:

[https://code4rena.com/reports/2022-06-notional-coop/#10-use-a-more-recent-version-of-solidity](https://code4rena.com/reports/2022-06-notional-coop/#10-use-a-more-recent-version-of-solidity)


-----

##  12. Use assembly to check for address(0)

Saves 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
 if iszero(_addr) {
  mstore(0x00, 'zero address')
  revert(0x00, 0x20)
 }
}
```
### Instances
[L2GraphTokenGateway.sol:L98](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L98)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:98:        require(_l2Router != address(0), "INVALID_L2_ROUTER");
```
[L2GraphTokenGateway.sol:L108](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L108)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:108:        require(_l1GRT != address(0), "INVALID_L1_GRT");
```
[L2GraphTokenGateway.sol:L118](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L118)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:118:        require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
```
[L2GraphTokenGateway.sol:L148](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L148)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:148:        require(_to != address(0), "INVALID_DESTINATION");
```
[L2GraphToken.sol:L49](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L49)
```
contracts/l2/token/L2GraphToken.sol:49:        require(_owner != address(0), "Owner must be set");
```
[L2GraphToken.sol:L60](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L60)
```
contracts/l2/token/L2GraphToken.sol:60:        require(_gw != address(0), "INVALID_GATEWAY");
```
[L2GraphToken.sol:L70](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L70)
```
contracts/l2/token/L2GraphToken.sol:70:        require(_addr != address(0), "INVALID_L1_ADDRESS");
```
[GraphTokenUpgradeable.sol:L106](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L106)
```
contracts/l2/token/GraphTokenUpgradeable.sol:106:        require(_account != address(0), "INVALID_MINTER");
```
[GraphProxy.sol:L105](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L105)
```
contracts/upgrades/GraphProxy.sol:105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```
[GraphProxy.sol:L143](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L143)
```
contracts/upgrades/GraphProxy.sol:143:            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
```
[L1GraphTokenGateway.sol:L74](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L74)
```
contracts/gateway/L1GraphTokenGateway.sol:74:        require(inbox != address(0), "INBOX_NOT_SET");
```
[L1GraphTokenGateway.sol:L110](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L110)
```
contracts/gateway/L1GraphTokenGateway.sol:110:        require(_inbox != address(0), "INVALID_INBOX");
```
[L1GraphTokenGateway.sol:L111](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L111)
```
contracts/gateway/L1GraphTokenGateway.sol:111:        require(_l1Router != address(0), "INVALID_L1_ROUTER");
```
[L1GraphTokenGateway.sol:L122](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L122)
```
contracts/gateway/L1GraphTokenGateway.sol:122:        require(_l2GRT != address(0), "INVALID_L2_GRT");
```
[L1GraphTokenGateway.sol:L132](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L132)
```
contracts/gateway/L1GraphTokenGateway.sol:132:        require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");
```
[L1GraphTokenGateway.sol:L142](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142)
```
contracts/gateway/L1GraphTokenGateway.sol:142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```
[L1GraphTokenGateway.sol:L153](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L153)
```
contracts/gateway/L1GraphTokenGateway.sol:153:        require(_newWhitelisted != address(0), "INVALID_ADDRESS");
```
[L1GraphTokenGateway.sol:L165](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L165)
```
contracts/gateway/L1GraphTokenGateway.sol:165:        require(_notWhitelisted != address(0), "INVALID_ADDRESS");
```
[L1GraphTokenGateway.sol:L202](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L202)
```
contracts/gateway/L1GraphTokenGateway.sol:202:        require(_to != address(0), "INVALID_DESTINATION");
```
[GraphTokenGateway.sol:L31](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L31)
```
contracts/gateway/GraphTokenGateway.sol:31:        require(_newPauseGuardian != address(0), "PauseGuardian must be set");
```
[Managed.sol:L104](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L104)
```
contracts/governance/Managed.sol:104:        require(_controller != address(0), "Controller must be set");
```
[Governed.sol:L41](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L41)
```
contracts/governance/Governed.sol:41:        require(_newGovernor != address(0), "Governor must be set");
```
[Governed.sol:L55](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L55)
```
contracts/governance/Governed.sol:55:            pendingGovernor != address(0) && msg.sender == pendingGovernor,
```

-----
## 13. Inline a modifier that’s only used once
### Description
As notDelegated() is only used once in this contract (in function proxiableUUID()), it should get inlined to save gas:
### Instances 1
[GraphTokenUpgradeable.sol:L59](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L59)
```
contracts/l2/token/GraphTokenUpgradeable.sol:59:    modifier onlyMinter() {
```
### Instances where modifiers is used only once
[GraphTokenUpgradeable.sol:L132](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L132)
```
contracts/l2/token/GraphTokenUpgradeable.sol:132:    function mint(address _to, uint256 _amount) external onlyMinter {
```
### Instances 2
[L2GraphTokenGateway.sol:L68](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L68)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:68:    modifier onlyL1Counterpart() {
```
### Instances where modifiers is used only once
[L2GraphTokenGateway.sol:L232](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L232)
```
contracts/l2/gateway/L2GraphTokenGateway.sol:232:    ) external payable override notPaused onlyL1Counterpart nonReentrant {
```
### Instances 3
[Managed.sol:L71](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L71)
```
contracts/governance/Managed.sol:71:    modifier onlyController() {
```
### Instances where modifiers is used only once
[Managed.sol:L95](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L95)
```
contracts/governance/Managed.sol:95:    function setController(address _controller) external onlyController {
```
### Instances 4
[L1GraphTokenGateway.sol:L73](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L73)
```
contracts/gateway/L1GraphTokenGateway.sol:73:    modifier onlyL2Counterpart() {
```
### Instances where modifiers is used only once
[L1GraphTokenGateway.sol:L269](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L269)
```
contracts/gateway/L1GraphTokenGateway.sol:269:    ) external payable override notPaused onlyL2Counterpart {
```
### Instances 5
[GraphTokenGateway.sol:L18](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L18)
```
contracts/gateway/GraphTokenGateway.sol:18:    modifier onlyGovernorOrGuardian() {
```
### Instances where modifiers is used only once
[GraphTokenGateway.sol:L47](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L47)
```
contracts/gateway/GraphTokenGateway.sol:47:    function setPaused(bool _newPaused) external onlyGovernorOrGuardian {
```
-----