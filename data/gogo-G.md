# 2022-10-THEGRAPH

## Gas Optimizations Report

### State variables should be cached in stack variables rather than re-reading them.

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

_There are **3** instances of this issue:_

```solidity
File: contracts/governance/Governed.sol

      /// @audit Cache `pendingGovernor`. Used 4 times in `acceptOwnership`
55:        pendingGovernor != address(0) && msg.sender == pendingGovernor,
60:    address oldPendingGovernor = pendingGovernor;
62:    governor = pendingGovernor;
66:    emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol

```solidity
File: contracts/governance/Pausable.sol

      /// @audit Cache `_partialPaused`. Used 3 times in `_setPartialPaused`
27:    if (_toPause == _partialPaused) {
31:    if (_partialPaused) {
34:    emit PartialPauseChanged(_partialPaused);

      /// @audit Cache `_paused`. Used 3 times in `_setPaused`
41:    if (_toPause == _paused) {
45:    if (_paused) {
48:    emit PauseChanged(_paused);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol

### `internal` and `private` functions that are called only once should be inlined.

The execution of a non-inlined function would cost up to 40 more gas because of two extra `jump`s as well as some other instructions.

_There are **3** instances of this issue:_

```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

290:  function parseOutboundData(bytes memory _data)
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

286:  function parseOutboundData(bytes memory _data) private view returns (address, bytes memory) {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

195:  function _getChainID() private pure returns (uint256) {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

### Using `!= 0` on `uints` costs less gas than `> 0`.

This change saves 3 gas per instance/loop

_There are **3** instances of this issue:_

```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

201:  require(_amount > 0, "INVALID_ZERO_AMOUNT");

217:          require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

146:  require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol

### Functions that are access-restricted from most users may be marked as `payable`

Marking a function as `payable` reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **6** instances of this issue:_

```solidity
File: contracts/governance/Governed.sol

40:   function transferOwnership(address _newGovernor) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol

```solidity
File: contracts/governance/Managed.sol

52:   function _onlyGovernor() internal view {

56:   function _onlyController() internal view {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol

```solidity
File: contracts/l2/token/L2GraphToken.sol

80:   function bridgeMint(address _account, uint256 _amount) external override onlyGateway {

90:   function bridgeBurn(address _account, uint256 _amount) external override onlyGateway {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol

```solidity
File: contracts/upgrades/GraphUpgradeable.sol

50:   function acceptProxy(IGraphProxy _proxy) external onlyProxyAdmin(_proxy) {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol

### Usage of `uint`s/`int`s smaller than 32 bytes (256 bits) incurs overhead

'When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.' \ https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html \ Use a larger size then downcast where needed

_There are **21** instances of this issue:_

```solidity
File: contracts/arbitrum/AddressAliasHelper.sol

29:   uint160 constant offset = uint160(0x1111000000000000000000000000000000001111);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/AddressAliasHelper.sol

```solidity
File: contracts/curation/ICuration.sol

10:   function setDefaultReserveRatio(uint32 _defaultReserveRatio) external;

14:   function setCurationTaxPercentage(uint32 _percentage) external;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

79:    uint8 _v,
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

```solidity
File: contracts/staking/IStaking.sol

32:   function setThawingPeriod(uint32 _thawingPeriod) external;

34:   function setCurationPercentage(uint32 _percentage) external;

36:   function setProtocolPercentage(uint32 _percentage) external;

38:   function setChannelDisputeEpochs(uint32 _channelDisputeEpochs) external;

40:   function setMaxAllocationEpochs(uint32 _maxAllocationEpochs) external;

42:   function setRebateRatio(uint32 _alphaNumerator, uint32 _alphaDenominator) external;

44:   function setDelegationRatio(uint32 _delegationRatio) external;

47:    uint32 _indexingRewardCut,

48:    uint32 _queryFeeCut,

49:    uint32 _cooldownBlocks

52:   function setDelegationParametersCooldown(uint32 _blocks) external;

54:   function setDelegationUnbondingPeriod(uint32 _delegationUnbondingPeriod) external;

56:   function setDelegationTaxPercentage(uint32 _percentage) external;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol

```solidity
File: contracts/staking/IStakingData.sol

37:     uint32 cooldownBlocks;

38:     uint32 indexingRewardCut;

39:     uint32 queryFeeCut;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStakingData.sol

```solidity
File: contracts/token/IGraphToken.sol

33:    uint8 _v,
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/token/IGraphToken.sol

### Don't compare boolean expressions to boolean literals

Use `if(x)`/`if(!x)` instead of `if(x == true)`/`if(x == false)`.

_There are **1** instances of this issue:_

```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

214:              extraData.length == 0 || callhookWhitelist[msg.sender] == true,
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

### Splitting `require()` statements that use `&&` saves gas

Instead of using `&&` on single `require` check using two `require` checks can save gas

_There are **3** instances of this issue:_

```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

142:  require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

```solidity
File: contracts/governance/Governed.sol

54:    require(
55:        pendingGovernor != address(0) && msg.sender == pendingGovernor,
56:        "Caller must be pending governor"
57:    );
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol

```solidity
File: contracts/upgrades/GraphProxy.sol

142:  require(
143:      _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
144:      "Caller must be the pending implementation"
145:  );
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol

### Use `calldata` instead of `memory` for function parameters

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **9** instances of this issue:_

```solidity
File: contracts/arbitrum/L1ArbitrumMessenger.sol

48:    L2GasParams memory _l2GasParams,

49:    bytes memory _data

75:    bytes memory _data
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol

```solidity
File: contracts/arbitrum/L2ArbitrumMessenger.sol

41:    bytes memory _data
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L2ArbitrumMessenger.sol

```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

290:  function parseOutboundData(bytes memory _data)

331:  bytes memory _data
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

```solidity
File: contracts/governance/Managed.sol

173:  function _syncContract(string memory _name) internal {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

266:  bytes memory _data

      /// @audit Store `_data` in calldata.
286:  function parseOutboundData(bytes memory _data) private view returns (address, bytes memory) {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol

### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for `>=` or `<=`. When using greater than or equal, two operations are performed: `>` and `=`. Using strict comparison operators hence saves gas

_There are **3** instances of this issue:_

```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

224:              require(msg.value >= expectedEth, "WRONG_ETH_VALUE");

275:  require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

95:    require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

### Tight variable packing

Solidity contracts have contiguous 32 bytes (256 bits) slots used in storage. By arranging the variables, it is possible to minimize the number of slots used within a contract’s storage and therefore reduce deployment costs.

_There are **1** instances of this issue:_

```solidity
File: contracts/governance/Pausable.sol

8:    /// @audit Currently storage variables are packed in 4 slots but could fit in 3
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol

### Use `immutable` & `constant` for state variables that do not change their value

_There are **1** instances of this issue:_

```solidity
File: contracts/governance/Managed.sol

29:   uint256[10] private __gap;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol

### `require()/revert()` strings longer than 32 bytes cost extra gas

Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas

_There are **6** instances of this issue:_

```solidity
File: contracts/gateway/GraphTokenGateway.sol

19:    require(
20:        msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
21:        "Only Governor or Guardian can call"
22:    );
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol

```solidity
File: contracts/governance/Managed.sol

53:    require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol

```solidity
File: contracts/upgrades/GraphProxy.sol

105:  require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

141:  require(Address.isContract(_pendingImplementation), "Implementation must be a contract");

142:  require(
143:      _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
144:      "Caller must be the pending implementation"
145:  );
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol

```solidity
File: contracts/upgrades/GraphUpgradeable.sol

32:    require(msg.sender == _implementation(), "Caller must be the implementation");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol

### Use custom errors rather than `revert()`/`require()` strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **61** instances of this issue:_

```solidity
File: contracts/arbitrum/L1ArbitrumMessenger.sol

100:  require(l2ToL1Sender != address(0), "NO_SENDER");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol

```solidity
File: contracts/gateway/GraphTokenGateway.sol

19:    require(
20:        msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
21:        "Only Governor or Guardian can call"
22:    );

31:    require(_newPauseGuardian != address(0), "PauseGuardian must be set");

40:    require(!_paused, "Paused (contract)");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol

```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

74:    require(inbox != address(0), "INBOX_NOT_SET");

78:    require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");

82:    require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");

110:  require(_inbox != address(0), "INVALID_INBOX");

111:  require(_l1Router != address(0), "INVALID_L1_ROUTER");

122:  require(_l2GRT != address(0), "INVALID_L2_GRT");

132:  require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");

142:  require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

153:  require(_newWhitelisted != address(0), "INVALID_ADDRESS");

154:  require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");

165:  require(_notWhitelisted != address(0), "INVALID_ADDRESS");

166:  require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");

200:  require(_l1Token == address(token), "TOKEN_NOT_GRT");

201:  require(_amount > 0, "INVALID_ZERO_AMOUNT");

202:  require(_to != address(0), "INVALID_DESTINATION");

213:          require(
214:              extraData.length == 0 || callhookWhitelist[msg.sender] == true,
215:              "CALL_HOOK_DATA_NOT_ALLOWED"
216:          );

217:          require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

224:              require(msg.value >= expectedEth, "WRONG_ETH_VALUE");

271:  require(_l1Token == address(token), "TOKEN_NOT_GRT");

275:  require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

```solidity
File: contracts/governance/Governed.sol

24:    require(msg.sender == governor, "Only Governor can call");

41:    require(_newGovernor != address(0), "Governor must be set");

54:    require(
55:        pendingGovernor != address(0) && msg.sender == pendingGovernor,
56:        "Caller must be pending governor"
57:    );
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol

```solidity
File: contracts/governance/Managed.sol

44:    require(!controller.paused(), "Paused");

45:    require(!controller.partialPaused(), "Partial-paused");

49:    require(!controller.paused(), "Paused");

53:    require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");

57:    require(msg.sender == address(controller), "Caller must be Controller");

104:  require(_controller != address(0), "Controller must be set");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

69:    require(
70:        msg.sender == AddressAliasHelper.applyL1ToL2Alias(l1Counterpart),
71:        "ONLY_COUNTERPART_GATEWAY"
72:    );

98:    require(_l2Router != address(0), "INVALID_L2_ROUTER");

108:  require(_l1GRT != address(0), "INVALID_L1_GRT");

118:  require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");

145:  require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

146:  require(_amount > 0, "INVALID_ZERO_AMOUNT");

147:  require(msg.value == 0, "INVALID_NONZERO_VALUE");

148:  require(_to != address(0), "INVALID_DESTINATION");

153:  require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");

233:  require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

234:  require(msg.value == 0, "INVALID_NONZERO_VALUE");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

60:    require(isMinter(msg.sender), "Only minter can call");

94:    require(_owner == recoveredAddress, "GRT: invalid permit");

95:    require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");

106:  require(_account != address(0), "INVALID_MINTER");

115:  require(_minters[_account], "NOT_A_MINTER");

123:  require(_minters[msg.sender], "NOT_A_MINTER");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

```solidity
File: contracts/l2/token/L2GraphToken.sol

36:    require(msg.sender == gateway, "NOT_GATEWAY");

49:    require(_owner != address(0), "Owner must be set");

60:    require(_gw != address(0), "INVALID_GATEWAY");

70:    require(_addr != address(0), "INVALID_L1_ADDRESS");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol

```solidity
File: contracts/upgrades/GraphProxy.sol

105:  require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

141:  require(Address.isContract(_pendingImplementation), "Implementation must be a contract");

142:  require(
143:      _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
144:      "Caller must be the pending implementation"
145:  );

157:  require(msg.sender != _admin(), "Cannot fallback to proxy target");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol

```solidity
File: contracts/upgrades/GraphProxyStorage.sol

62:    require(msg.sender == _admin(), "Caller must be admin");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol

```solidity
File: contracts/upgrades/GraphUpgradeable.sol

24:    require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");

32:    require(msg.sender == _implementation(), "Caller must be the implementation");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol

### Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

It is expected that the value should be converted into a constant value at compile time. But actually the expression is re-calculated each time the constant is referenced.

_There are **2** instances of this issue:_

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

38:   bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");

39:   bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

### Using bools for storage variables incurs overhead

Use `uint256(1)` and `uint256(2)` for `true`/`false` to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from 'false' to 'true', after having been 'true' in the past

_There are **2** instances of this issue:_

```solidity
File: contracts/governance/Pausable.sol

8:    bool internal _partialPaused;

10:   bool internal _paused;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol