### [G-01] `!=` cost 6 gas less than `>` in require

Recommend to use the `!=` for comparisons with 0 in require statemnent. This is type of gas optimization is 6 gas cheaper than `>`

_There have **3** instance of this issues:_

File: L1GraphTokenGateway.sol lines 201, 217

```
require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol#L201

```
require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol#L217

File: L2GraphTokenGateway.sol line 146

```
2022-10-thegraph => require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol#L146

### [G-02] Recommend to use short require string to save deployment gas

If you use longer require strings than 32 bytes that will cost more expensive than short require string.

_There are **8** instances of this issue:_

File: GraphTokenGateway.sol line 21

```
"Only Governor or Guardian can call"
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/GraphTokenGateway.sol#L21

File: Managed.sol line 53

```
require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Managed.sol#L53

File: GraphProxy.sol lines 105, 141, 144

```
require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxy.sol#L105

```
require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxy.sol#L141

```
"Caller must be the pending implementation"
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxy.sol#L144

File: GraphUpgradeable.sol line 32

```
require(msg.sender == _implementation(), "Caller must be the implementation");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphUpgradeable.sol#L32

### [G-03] Recommend to use `uint256(1)`/`uint256(2)` for true and false

If you don't use boolean for storage you will avoid Gwarmaccess 100 gas. Also boolean from true to false cost 20000 gas rather than uint256(2) to uint256(1) that cost less.

_There have **3** instance of this issues:_

File: L1GraphTokenGateway.sol line 35

```
mapping(address => bool) public callhookWhitelist;
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol#L35

File: Pausable.sol lines 8, 10

```
bool internal _partialPaused;
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Pausable.sol#L8

```
bool internal _paused;
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Pausable.sol#L10

### [G-04] Using of custom errors than are came from 0.8.4 instead of `require()`/`revert()` strings will save deployment gas

Use more recent version of solidity compiler and take advantage of custom errors and save from deployment gas and when condtions are done for reverting.

_There have **64** instance of this issues:_

File: GraphTokenGateway.sol lines 19,31,40

```
require(
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/GraphTokenGateway.sol/#L19

```
require(_newPauseGuardian != address(0), "PauseGuardian must be set");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/GraphTokenGateway.sol/#L31

```
require(!_paused, "Paused (contract)");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/GraphTokenGateway.sol/#L40

File: L1GraphTokenGateway.sol lines 74,78,82,110,111,122,132,142,153,154,165,166,200,201,202,213,217,224,271,275

```
require(inbox != address(0), "INBOX_NOT_SET");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L74

```
require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L78

```
require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L82

```
require(_inbox != address(0), "INVALID_INBOX");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L110

```
require(_l1Router != address(0), "INVALID_L1_ROUTER");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L111

```
require(_l2GRT != address(0), "INVALID_L2_GRT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L122

```
require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L132

```
require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L142

```
require(_newWhitelisted != address(0), "INVALID_ADDRESS");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L153

```
require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L154

```
require(_notWhitelisted != address(0), "INVALID_ADDRESS");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L165

```
require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L166

```
require(_l1Token == address(token), "TOKEN_NOT_GRT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L200

```
require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L201

```
require(_to != address(0), "INVALID_DESTINATION");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L202

```
require(
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L213

```
require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L217

```
require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L224

```
require(_l1Token == address(token), "TOKEN_NOT_GRT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L271

```
require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol/#L275

File: Governed.sol lines 24,41,54

```
require(msg.sender == governor, "Only Governor can call");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Governed.sol/#L24

```
require(_newGovernor != address(0), "Governor must be set");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Governed.sol/#L41

```
require(
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Governed.sol/#L54

File: Managed.sol lines 44,45,49,53,57,104

```
require(!controller.paused(), "Paused");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Managed.sol/#L44

```
require(!controller.partialPaused(), "Partial-paused");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Managed.sol/#L45

```
require(!controller.paused(), "Paused");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Managed.sol/#L49

```
require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Managed.sol/#L53

```
require(msg.sender == address(controller), "Caller must be Controller");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Managed.sol/#L57

```
require(_controller != address(0), "Controller must be set");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/governance/Managed.sol/#L104

File: L2GraphTokenGateway.sol lines 69,98,108,118,145,146,147,148,153,233,234

```
require(
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L69

```
require(_l2Router != address(0), "INVALID_L2_ROUTER");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L98

```
require(_l1GRT != address(0), "INVALID_L1_GRT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L108

```
require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L118

```
require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L145

```
require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L146

```
require(msg.value == 0, "INVALID_NONZERO_VALUE");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L147

```
require(_to != address(0), "INVALID_DESTINATION");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L148

```
require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L153

```
require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L233

```
require(msg.value == 0, "INVALID_NONZERO_VALUE");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol/#L234

File: GraphTokenUpgradeable.sol lines 60,94,95,106,115,123

```
require(isMinter(msg.sender), "Only minter can call");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/token/GraphTokenUpgradeable.sol/#L60

```
require(_owner == recoveredAddress, "GRT: invalid permit");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/token/GraphTokenUpgradeable.sol/#L94

```
require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/token/GraphTokenUpgradeable.sol/#L95

```
require(_account != address(0), "INVALID_MINTER");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/token/GraphTokenUpgradeable.sol/#L106

```
require(_minters[_account], "NOT_A_MINTER");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/token/GraphTokenUpgradeable.sol/#L115

```
require(_minters[msg.sender], "NOT_A_MINTER");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/token/GraphTokenUpgradeable.sol/#L123

File: L2GraphToken.sol lines 36,49,60,70

```
require(msg.sender == gateway, "NOT_GATEWAY");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/token/L2GraphToken.sol/#L36

```
require(_owner != address(0), "Owner must be set");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/token/L2GraphToken.sol/#L49

```
require(_gw != address(0), "INVALID_GATEWAY");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/token/L2GraphToken.sol/#L60

```
require(_addr != address(0), "INVALID_L1_ADDRESS");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/token/L2GraphToken.sol/#L70

File: GraphProxy.sol lines 105,133,141,142,157

```
require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxy.sol/#L105

```
require(success);
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxy.sol/#L133

```
require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxy.sol/#L141

```
require(
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxy.sol/#L142

```
require(msg.sender != _admin(), "Cannot fallback to proxy target");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxy.sol/#L157

File: GraphProxyAdmin.sol lines 34,47,59

```
require(success);
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxyAdmin.sol/#L34

```
require(success);
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxyAdmin.sol/#L47

```
require(success);
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxyAdmin.sol/#L59

File: GraphProxyStorage.sol line 62

```
require(msg.sender == _admin(), "Caller must be admin");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxyStorage.sol/#L62

File: GraphUpgradeable.sol lines 24,32

```
require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphUpgradeable.sol/#L24

```
require(msg.sender == _implementation(), "Caller must be the implementation");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphUpgradeable.sol/#L32

### [G-05] Recommending to not use constructor parameters where is possible because of their cost.

If you not use constructor parameters, deployment will be cheaper. I recomend instead of them to hard code them and change in time if needed with custome function for that purpose.

_There have **1** instance of this issues:_

File: GraphProxy.sol line 46

```
constructor(address _impl, address _admin) {
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/upgrades/GraphProxy.sol#L46

### [G-06] Recommend to use one operator for comparison

If you use `>=` or `<=` this will cost more gas because in the EVM there is no implementation of Opcodes for `>=` and `<= and two operations are done.

_There have **3** instance of this issues:_

File: L1GraphTokenGateway.sol line 224, 275

```
require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/gateway/L1GraphTokenGateway.sol#L224

```
require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529fc4udit/2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol/#L275

File: GraphTokenUpgradeable.sol line 95

```
require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529fc4udit/2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol/#L95

### [G‑07] Recommend to change memory to calldata for read-only arguments for functions that are called extnally

_There have **1** instance of this issues:_

File: L2GraphTokenGateway.sol line 286

```
function parseOutboundData(bytes memory _data)
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529f/contracts/l2/gateway/L2GraphTokenGateway.sol#L286
