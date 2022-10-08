# GAS OPTIMIZATIONS REPORT

## THEGRAPH

### Use `1` and `2` instead of `true` and `false`

EVM word size is 32 bytes. The `uint256 1` and `uint256 2` will be cheaper to use than the `boolean true` and `boolean false` since the first are 32 bytes long and the last are only 1 byte

[Pausable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol):

```solidity
line#L8:   bool internal _partialPaused;

line#L10:  bool internal _paused;
```

### Function inlining

If a function is called only once it can be inlined in order to save 20 gas

[GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol):

```solidity
line#L195: function _getChainID() private pure returns (uint256) {
```

[L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol):

```solidity
line#L290: function parseOutboundData(bytes memory _data)
```

[L2GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol):

```solidity
line#L286: function parseOutboundData(bytes memory _data) private view returns (address, bytes memory) {
```

### Pack storage variables in less slots to save gas

[Pausable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol):

```solidity
line#L8:   /// @audit Currently storage variables are packed in 4 slots but could fit in 3
```

### Mark functions as `payable` to avoid the low-level evm `is-a-payment-provided` check

The EVM checks whether a payment is provided to the function and this check costs additional gas. If the function is marked as `payable` there is no such check

[Managed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol):

```solidity
line#L52:  function _onlyGovernor() internal view {

line#L56:  function _onlyController() internal view {
```

[Governed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol):

```solidity
line#L40:  function transferOwnership(address _newGovernor) external onlyGovernor {
```

[GraphUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol):

```solidity
line#L50:  function acceptProxy(IGraphProxy _proxy) external onlyProxyAdmin(_proxy) {
```

[L2GraphToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol):

```solidity
line#L80:  function bridgeMint(address _account, uint256 _amount) external override onlyGateway {

line#L90:  function bridgeBurn(address _account, uint256 _amount) external override onlyGateway {
```

### Use `constant` and `immutable` keywords for storage varibles if they doesn't change through contract's lifecycle

That way the variable will go to the contract's bytecode and will not allocate a storage slot

[Managed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol):

```solidity
line#L29:  uint256[10] private __gap;
```

### Use `calldata` where possible instead of `memory` to save gas

[L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol):

```solidity
line#L290: function parseOutboundData(bytes memory _data)

line#L331: bytes memory _data
```

[L2GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol):

```solidity
line#L266: bytes memory _data

           /// @audit Store `_data` in calldata.
line#L286: function parseOutboundData(bytes memory _data) private view returns (address, bytes memory) {
```

[L2ArbitrumMessenger.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L2ArbitrumMessenger.sol):

```solidity
line#L41:  bytes memory _data
```

[Managed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol):

```solidity
line#L173: function _syncContract(string memory _name) internal {
```

[L1ArbitrumMessenger.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol):

```solidity
line#L48:  L2GasParams memory _l2GasParams,

line#L49:  bytes memory _data

line#L75:  bytes memory _data
```

### `x < y + 1` is cheaper than `x <= y`

[L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol):

```solidity
line#L224: require(msg.value >= expectedEth, "WRONG_ETH_VALUE");

line#L275: require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
```

[GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol):

```solidity
line#L95:  require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```

### Use `uint256` instead of the smaller uints where possible

The word-size in ethereum is 32 bytes which means that every variables which is sized with less bytes will cost more to operate with

[GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol):

```solidity
line#L79:  uint8 _v,
```

[IGraphToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/token/IGraphToken.sol):

```solidity
line#L33:  uint8 _v,
```

[IStaking.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol):

```solidity
line#L32:  function setThawingPeriod(uint32 _thawingPeriod) external;

line#L34:  function setCurationPercentage(uint32 _percentage) external;

line#L38:  function setChannelDisputeEpochs(uint32 _channelDisputeEpochs) external;

line#L40:  function setMaxAllocationEpochs(uint32 _maxAllocationEpochs) external;

line#L44:  function setDelegationRatio(uint32 _delegationRatio) external;

line#L47:  uint32 _indexingRewardCut,

line#L49:  uint32 _cooldownBlocks

line#L52:  function setDelegationParametersCooldown(uint32 _blocks) external;

line#L56:  function setDelegationTaxPercentage(uint32 _percentage) external;
```

[IStakingData.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStakingData.sol):

```solidity
line#L37:  uint32 cooldownBlocks;

line#L38:  uint32 indexingRewardCut;

line#L39:  uint32 queryFeeCut;
```

[AddressAliasHelper.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/AddressAliasHelper.sol):

```solidity
line#L29:  uint160 constant offset = uint160(0x1111000000000000000000000000000000001111);
```

[ICuration.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol):

```solidity
line#L10:  function setDefaultReserveRatio(uint32 _defaultReserveRatio) external;

line#L14:  function setCurationTaxPercentage(uint32 _percentage) external;
```

### Use custom errors instead of `require` strings

[Governed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol):

```solidity
line#L24:  require(msg.sender == governor, "Only Governor can call");

line#L41:  require(_newGovernor != address(0), "Governor must be set");

line#L54:  require(
line#L55:  pendingGovernor != address(0) && msg.sender == pendingGovernor,
line#L56:  "Caller must be pending governor"
line#L57:  );
```

[GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol):

```solidity
line#L60:  require(isMinter(msg.sender), "Only minter can call");

line#L94:  require(_owner == recoveredAddress, "GRT: invalid permit");

line#L106: require(_account != address(0), "INVALID_MINTER");

line#L115: require(_minters[_account], "NOT_A_MINTER");
```

[GraphUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol):

```solidity
line#L24:  require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");

line#L32:  require(msg.sender == _implementation(), "Caller must be the implementation");
```

[GraphProxyStorage.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol):

```solidity
line#L62:  require(msg.sender == _admin(), "Caller must be admin");
```

[L1ArbitrumMessenger.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol):

```solidity
line#L100: require(l2ToL1Sender != address(0), "NO_SENDER");
```

[L2GraphToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol):

```solidity
line#L36:  require(msg.sender == gateway, "NOT_GATEWAY");

line#L49:  require(_owner != address(0), "Owner must be set");

line#L60:  require(_gw != address(0), "INVALID_GATEWAY");

line#L70:  require(_addr != address(0), "INVALID_L1_ADDRESS");
```

[L2GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol):

```solidity
line#L69:  require(
line#L70:  msg.sender == AddressAliasHelper.applyL1ToL2Alias(l1Counterpart),
line#L71:  "ONLY_COUNTERPART_GATEWAY"
line#L72:  );

line#L98:  require(_l2Router != address(0), "INVALID_L2_ROUTER");

line#L118: require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");

line#L145: require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

line#L147: require(msg.value == 0, "INVALID_NONZERO_VALUE");

line#L148: require(_to != address(0), "INVALID_DESTINATION");

line#L233: require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

line#L234: require(msg.value == 0, "INVALID_NONZERO_VALUE");
```

[Managed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol):

```solidity
line#L44:  require(!controller.paused(), "Paused");

line#L45:  require(!controller.partialPaused(), "Partial-paused");

line#L53:  require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");

line#L57:  require(msg.sender == address(controller), "Caller must be Controller");
```

[GraphProxy.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol):

```solidity
line#L105: require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

line#L141: require(Address.isContract(_pendingImplementation), "Implementation must be a contract");

line#L142: require(
line#L143: _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
line#L144: "Caller must be the pending implementation"
line#L145: );

line#L157: require(msg.sender != _admin(), "Cannot fallback to proxy target");
```

[GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol):

```solidity
line#L19:  require(
line#L20:  msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
line#L21:  "Only Governor or Guardian can call"
line#L22:  );

line#L31:  require(_newPauseGuardian != address(0), "PauseGuardian must be set");

line#L40:  require(!_paused, "Paused (contract)");
```

[L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol):

```solidity
line#L74:  require(inbox != address(0), "INBOX_NOT_SET");

line#L78:  require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");

line#L110: require(_inbox != address(0), "INVALID_INBOX");

line#L111: require(_l1Router != address(0), "INVALID_L1_ROUTER");

line#L132: require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");

line#L142: require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

line#L154: require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");

line#L165: require(_notWhitelisted != address(0), "INVALID_ADDRESS");

line#L200: require(_l1Token == address(token), "TOKEN_NOT_GRT");

line#L201: require(_amount > 0, "INVALID_ZERO_AMOUNT");

line#L213: require(
line#L214: extraData.length == 0 || callhookWhitelist[msg.sender] == true,
line#L215: "CALL_HOOK_DATA_NOT_ALLOWED"
line#L216: );

line#L217: require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

line#L271: require(_l1Token == address(token), "TOKEN_NOT_GRT");

line#L275: require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
```

### Use `if (x != 0)` instead of `if (x > 0)` for uints comparison

Saves 3 gas per `if`-check

[L2GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol):

```solidity
line#L146: require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

[L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol):

```solidity
line#L201: require(_amount > 0, "INVALID_ZERO_AMOUNT");

line#L217: require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```

### Do not use strings longer than 32 bytes

[GraphProxy.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol):

```solidity
line#L105: require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

line#L141: require(Address.isContract(_pendingImplementation), "Implementation must be a contract");

line#L142: require(
line#L143: _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
line#L144: "Caller must be the pending implementation"
line#L145: );
```

[GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol):

```solidity
line#L19:  require(
line#L20:  msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
line#L21:  "Only Governor or Guardian can call"
line#L22:  );
```

[Managed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol):

```solidity
line#L53:  require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```

[GraphUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol):

```solidity
line#L32:  require(msg.sender == _implementation(), "Caller must be the implementation");
```

### Avoid using `&&` in `require()` or `if()` statements

Use multiple `require()`/`if` statements instead

[GraphProxy.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol):

```solidity
line#L142: require(
line#L143: _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
line#L144: "Caller must be the pending implementation"
line#L145: );
```

[L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol):

```solidity
line#L142: require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```

[Governed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol):

```solidity
line#L54:  require(
line#L55:  pendingGovernor != address(0) && msg.sender == pendingGovernor,
line#L56:  "Caller must be pending governor"
line#L57:  );
```

### Cache storage variables in memory

Optimization comes from using MSTORE and MLOAD instead of SSTORE and SLOAD

[Governed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol):

```solidity
           /// @audit Cache `pendingGovernor`. Used 4 times in `acceptOwnership`
line#L55:  pendingGovernor != address(0) && msg.sender == pendingGovernor,
line#L60:  address oldPendingGovernor = pendingGovernor;
line#L62:  governor = pendingGovernor;
line#L66:  emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
```

[Pausable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol):

```solidity
           /// @audit Cache `_partialPaused`. Used 3 times in `_setPartialPaused`
line#L27:  if (_toPause == _partialPaused) {
line#L31:  if (_partialPaused) {
line#L34:  emit PartialPauseChanged(_partialPaused);

           /// @audit Cache `_paused`. Used 3 times in `_setPaused`
line#L41:  if (_toPause == _paused) {
line#L45:  if (_paused) {
line#L48:  emit PauseChanged(_paused);
```

### If a `constant` variable initialization includes `keccak256()`, `sha256()`, etc. it should be marked as `immutable` instead

The result of the calculation of the hash (or whatever the expression is) is expected to be saved once compiled, but that's not the case. If marked as `constant` the calculation will be executed every time the variable is accessed.

[GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol):

```solidity
line#L38:  bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");

line#L39:  bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
```

### Do not compare a boolean to `true` or `false`

Directly use the boolean

[L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol):

```solidity
line#L214: extraData.length == 0 || callhookWhitelist[msg.sender] == true,
```
