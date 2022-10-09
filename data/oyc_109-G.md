## [G-01] Don't Initialize Variables with Default Value

Uninitialized variables are assigned with the types default value. Explicitly initializing a variable with it's default value costs unnecesary gas.

```
2022-10-thegraph/contracts/rewards/RewardsManager.sol::161 => for (uint256 i = 0; i < _subgraphDeploymentID.length; i++) {
2022-10-thegraph/contracts/staking/Staking.sol::924 => for (uint256 i = 0; i < _requests.length; i++) {
2022-10-thegraph/contracts/staking/Staking.sol::979 => uint256 curationFees = 0;
2022-10-thegraph/contracts/staking/Staking.sol::1050 => for (uint256 i = 0; i < _allocationID.length; i++) {
2022-10-thegraph/contracts/staking/Staking.sol::1454 => uint256 delegationRewards = 0;
2022-10-thegraph/contracts/staking/Staking.sol::1475 => uint256 delegationRewards = 0;
2022-10-thegraph/contracts/staking/libs/Rebates.sol::87 => uint256 rebateReward = 0;
```

## [G-02] Cache Array Length Outside of Loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

```
2022-10-thegraph/contracts/rewards/RewardsManager.sol::161 => for (uint256 i = 0; i < _subgraphDeploymentID.length; i++) {
2022-10-thegraph/contracts/staking/Staking.sol::924 => for (uint256 i = 0; i < _requests.length; i++) {
2022-10-thegraph/contracts/staking/Staking.sol::1050 => for (uint256 i = 0; i < _allocationID.length; i++) {
```

## [G-03] Using > 0 costs more gas than != 0 when used on a uint in a require() statement

When dealing with unsigned integer types, comparisons with != 0 are cheaper then with > 0. This change saves 6 gas per instance

```
2022-10-thegraph/contracts/curation/Curation.sol::109 => require(_defaultReserveRatio > 0, "Default reserve ratio must be > 0");
2022-10-thegraph/contracts/curation/Curation.sol::138 => require(_minimumCurationDeposit > 0, "Minimum curation deposit cannot be 0");
2022-10-thegraph/contracts/curation/Curation.sol::223 => require(_tokensIn > 0, "Cannot deposit zero tokens");
2022-10-thegraph/contracts/curation/Curation.sol::283 => require(_signalIn > 0, "Cannot burn zero signal");
2022-10-thegraph/contracts/epochs/EpochManager.sol::28 => require(_epochLength > 0, "Epoch length cannot be 0");
2022-10-thegraph/contracts/epochs/EpochManager.sol::47 => require(_epochLength > 0, "Epoch length cannot be 0");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::201 => require(_amount > 0, "INVALID_ZERO_AMOUNT");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::217 => require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::146 => require(_amount > 0, "INVALID_ZERO_AMOUNT");
2022-10-thegraph/contracts/staking/Staking.sol::254 => require(_minimumIndexerStake > 0, "!minimumIndexerStake");
2022-10-thegraph/contracts/staking/Staking.sol::272 => require(_thawingPeriod > 0, "!thawingPeriod");
2022-10-thegraph/contracts/staking/Staking.sol::328 => require(_channelDisputeEpochs > 0, "!channelDisputeEpochs");
2022-10-thegraph/contracts/staking/Staking.sol::369 => require(_alphaNumerator > 0 && _alphaDenominator > 0, "!alpha");
2022-10-thegraph/contracts/staking/Staking.sol::486 => require(_delegationUnbondingPeriod > 0, "!delegationUnbondingPeriod");
2022-10-thegraph/contracts/staking/Staking.sol::694 => require(_tokens > 0, "!tokens");
2022-10-thegraph/contracts/staking/Staking.sol::721 => require(indexerStake.tokensStaked > 0, "!stake");
2022-10-thegraph/contracts/staking/Staking.sol::725 => require(tokensToLock > 0, "!stake-avail");
2022-10-thegraph/contracts/staking/Staking.sol::776 => require(_tokens > 0, "!tokens");
2022-10-thegraph/contracts/staking/Staking.sol::782 => require(indexerStake.tokensStaked > 0, "!stake");
2022-10-thegraph/contracts/staking/Staking.sol::1082 => require(tokensToWithdraw > 0, "!tokens");
2022-10-thegraph/contracts/staking/Staking.sol::1185 => require(epochs > 0, "<epochs");
2022-10-thegraph/contracts/staking/Staking.sol::1323 => require(_tokens > 0, "!tokens");
2022-10-thegraph/contracts/staking/Staking.sol::1327 => require(stakes[_indexer].tokensStaked > 0, "!stake");
2022-10-thegraph/contracts/staking/Staking.sol::1341 => require(shares > 0, "!shares");
2022-10-thegraph/contracts/staking/Staking.sol::1368 => require(_shares > 0, "!shares");
2022-10-thegraph/contracts/staking/Staking.sol::1422 => require(tokensToWithdraw > 0, "!tokens");
```

## [G-04] Long Revert Strings

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

```
2022-10-thegraph/contracts/curation/Curation.sol::109 => require(_defaultReserveRatio > 0, "Default reserve ratio must be > 0");
2022-10-thegraph/contracts/curation/Curation.sol::138 => require(_minimumCurationDeposit > 0, "Minimum curation deposit cannot be 0");
2022-10-thegraph/contracts/curation/Curation.sol::195 => require(msg.sender == address(staking()), "Caller must be the staking contract");
2022-10-thegraph/contracts/epochs/EpochManager.sol::48 => require(_epochLength != epochLength, "Epoch length must be different to current");
2022-10-thegraph/contracts/epochs/EpochManager.sol::95 => require(_block < currentBlock, "Can only retrieve past block hashes");
2022-10-thegraph/contracts/governance/Managed.sol::53 => require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
2022-10-thegraph/contracts/rewards/RewardsManager.sol::95 => require(_issuanceRate >= MIN_ISSUANCE_RATE, "Issuance rate under minimum allowed");
2022-10-thegraph/contracts/rewards/RewardsManager.sol::411 => require(msg.sender == address(staking), "Caller must be the staking contract");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::105 => require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::141 => require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::32 => require(msg.sender == _implementation(), "Caller must be the implementation");
```

## [G-05] Use calldata instead of memory

Use calldata instead of memory for function parameters saves gas if the function argument is only read.

```
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::286 => function parseOutboundData(bytes memory _data) private view returns (address, bytes memory) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::126 => function tokensUsed(Stakes.Indexer memory stake) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::137 => function tokensSecureStake(Stakes.Indexer memory stake) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::149 => function tokensAvailable(Stakes.Indexer memory stake) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::189 => function tokensWithdrawable(Stakes.Indexer memory stake) internal view returns (uint256) {
```

## [G-06] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost

```
2022-10-thegraph/contracts/curation/Curation.sol::98 => function setDefaultReserveRatio(uint32 _defaultReserveRatio) external override onlyGovernor {
2022-10-thegraph/contracts/curation/Curation.sol::148 => function setCurationTaxPercentage(uint32 _percentage) external override onlyGovernor {
2022-10-thegraph/contracts/curation/Curation.sol::170 => function setCurationTokenMaster(address _curationTokenMaster) external override onlyGovernor {
2022-10-thegraph/contracts/curation/GraphCurationToken.sol::36 => function mint(address _to, uint256 _amount) public onlyGovernor {
2022-10-thegraph/contracts/curation/GraphCurationToken.sol::45 => function burnFrom(address _account, uint256 _amount) public onlyGovernor {
2022-10-thegraph/contracts/epochs/EpochManager.sol::27 => function initialize(address _controller, uint256 _epochLength) external onlyImpl {
2022-10-thegraph/contracts/epochs/EpochManager.sol::46 => function setEpochLength(uint256 _epochLength) external override onlyGovernor {
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::20 => function initialize(address _controller) external onlyImpl {
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::28 => function approveAll(address _spender) external onlyGovernor {
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::36 => function revokeAll(address _spender) external onlyGovernor {
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::30 => function setPauseGuardian(address _newPauseGuardian) external onlyGovernor {
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::47 => function setPaused(bool _newPaused) external onlyGovernorOrGuardian {
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::99 => function initialize(address _controller) external onlyImpl {
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::109 => function setArbitrumAddresses(address _inbox, address _l1Router) external onlyGovernor {
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::121 => function setL2TokenAddress(address _l2GRT) external onlyGovernor {
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::131 => function setL2CounterpartAddress(address _l2Counterpart) external onlyGovernor {
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::141 => function setEscrowAddress(address _escrow) external onlyGovernor {
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::152 => function addToCallhookWhitelist(address _newWhitelisted) external onlyGovernor {
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::164 => function removeFromCallhookWhitelist(address _notWhitelisted) external onlyGovernor {
2022-10-thegraph/contracts/governance/Controller.sol::69 => function unsetContractProxy(bytes32 _id) external override onlyGovernor {
2022-10-thegraph/contracts/governance/Controller.sol::87 => function updateController(bytes32 _id, address _controller) external override onlyGovernor {
2022-10-thegraph/contracts/governance/Controller.sol::98 => function setPartialPaused(bool _partialPaused) external override onlyGovernorOrGuardian {
2022-10-thegraph/contracts/governance/Controller.sol::106 => function setPaused(bool _paused) external override onlyGovernorOrGuardian {
2022-10-thegraph/contracts/governance/Controller.sol::114 => function setPauseGuardian(address _newPauseGuardian) external override onlyGovernor {
2022-10-thegraph/contracts/governance/Governed.sol::40 => function transferOwnership(address _newGovernor) external onlyGovernor {
2022-10-thegraph/contracts/governance/GraphGovernance.sol::38 => function initialize(address _governor) public onlyImpl {
2022-10-thegraph/contracts/governance/Managed.sol::95 => function setController(address _controller) external onlyController {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::87 => function initialize(address _controller) external onlyImpl {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::97 => function setL2Router(address _l2Router) external onlyGovernor {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::107 => function setL1TokenAddress(address _l1GRT) external onlyGovernor {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::117 => function setL1CounterpartAddress(address _l1Counterpart) external onlyGovernor {
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::105 => function addMinter(address _account) external onlyGovernor {
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::114 => function removeMinter(address _account) external onlyGovernor {
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::132 => function mint(address _to, uint256 _amount) external onlyMinter {
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::48 => function initialize(address _owner) external onlyImpl {
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::59 => function setGateway(address _gw) external onlyGovernor {
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::69 => function setL1Address(address _addr) external onlyGovernor {
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::80 => function bridgeMint(address _account, uint256 _amount) external override onlyGateway {
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::90 => function bridgeBurn(address _account, uint256 _amount) external override onlyGateway {
2022-10-thegraph/contracts/rewards/RewardsManager.sol::72 => function initialize(address _controller) external onlyImpl {
2022-10-thegraph/contracts/rewards/RewardsManager.sol::86 => function setIssuanceRate(uint256 _issuanceRate) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::245 => function setMinimumIndexerStake(uint256 _minimumIndexerStake) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::263 => function setThawingPeriod(uint32 _thawingPeriod) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::281 => function setCurationPercentage(uint32 _percentage) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::300 => function setProtocolPercentage(uint32 _percentage) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::319 => function setChannelDisputeEpochs(uint32 _channelDisputeEpochs) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::337 => function setMaxAllocationEpochs(uint32 _maxAllocationEpochs) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::381 => function setDelegationRatio(uint32 _delegationRatio) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::456 => function setDelegationParametersCooldown(uint32 _blocks) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::495 => function setDelegationTaxPercentage(uint32 _percentage) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::515 => function setSlasher(address _slasher, bool _allowed) external override onlyGovernor {
2022-10-thegraph/contracts/staking/Staking.sol::526 => function setAssetHolder(address _assetHolder, bool _allowed) external override onlyGovernor {
2022-10-thegraph/contracts/token/GraphToken.sol::127 => function addMinter(address _account) external onlyGovernor {
2022-10-thegraph/contracts/token/GraphToken.sol::135 => function removeMinter(address _account) external onlyGovernor {
2022-10-thegraph/contracts/token/GraphToken.sol::151 => function mint(address _to, uint256 _amount) external onlyMinter {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::68 => function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::77 => function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::86 => function acceptProxy(GraphUpgradeable _implementation, IGraphProxy _proxy) public onlyGovernor {
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::50 => function acceptProxy(IGraphProxy _proxy) external onlyProxyAdmin(_proxy) {
```

## [G-07] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```
2022-10-thegraph/contracts/curation/Curation.sol::33 => uint32 private constant MAX_PPM = 1000000;
2022-10-thegraph/contracts/curation/CurationStorage.sol::12 => uint32 reserveRatio; // Ratio for the bonding curve
2022-10-thegraph/contracts/curation/CurationStorage.sol::20 => uint32 public override curationTaxPercentage;
2022-10-thegraph/contracts/curation/CurationStorage.sol::24 => uint32 public defaultReserveRatio;
2022-10-thegraph/contracts/staking/IStakingData.sol::37 => uint32 cooldownBlocks; // Blocks to wait before updating parameters
2022-10-thegraph/contracts/staking/IStakingData.sol::38 => uint32 indexingRewardCut; // in PPM
2022-10-thegraph/contracts/staking/IStakingData.sol::39 => uint32 queryFeeCut; // in PPM
2022-10-thegraph/contracts/staking/Staking.sol::30 => uint32 private constant MAX_PPM = 1000000;
2022-10-thegraph/contracts/staking/StakingStorage.sol::18 => uint32 public thawingPeriod; // in blocks
2022-10-thegraph/contracts/staking/StakingStorage.sol::22 => uint32 public curationPercentage;
2022-10-thegraph/contracts/staking/StakingStorage.sol::26 => uint32 public protocolPercentage;
2022-10-thegraph/contracts/staking/StakingStorage.sol::29 => uint32 public channelDisputeEpochs;
2022-10-thegraph/contracts/staking/StakingStorage.sol::32 => uint32 public maxAllocationEpochs;
2022-10-thegraph/contracts/staking/StakingStorage.sol::35 => uint32 public alphaNumerator;
2022-10-thegraph/contracts/staking/StakingStorage.sol::36 => uint32 public alphaDenominator;
2022-10-thegraph/contracts/staking/StakingStorage.sol::60 => uint32 public delegationRatio;
2022-10-thegraph/contracts/staking/StakingStorage.sol::63 => uint32 public delegationParametersCooldown;
2022-10-thegraph/contracts/staking/StakingStorage.sol::66 => uint32 public delegationUnbondingPeriod; // in epochs
2022-10-thegraph/contracts/staking/StakingStorage.sol::70 => uint32 public delegationTaxPercentage;
2022-10-thegraph/contracts/staking/libs/Rebates.sol::26 => uint32 unclaimedAllocationsCount; // amount of unclaimed allocations
2022-10-thegraph/contracts/staking/libs/Rebates.sol::27 => uint32 alphaNumerator; // numerator of `alpha` in the cobb-douglas function
2022-10-thegraph/contracts/staking/libs/Rebates.sol::28 => uint32 alphaDenominator; // denominator of `alpha` in the cobb-douglas function
```

## [G-08] Using bools for storage incurs overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.
Use uint256(1) and uint256(2) for true/false instead

```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::35 => mapping(address => bool) public callhookWhitelist;
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::51 => mapping(address => bool) private _minters;
2022-10-thegraph/contracts/staking/StakingStorage.sol::53 => mapping(address => bool) public slashers;
2022-10-thegraph/contracts/staking/StakingStorage.sol::78 => mapping(address => mapping(address => bool)) public operatorAuth;
2022-10-thegraph/contracts/staking/StakingStorage.sol::83 => mapping(address => bool) public assetHolders;
2022-10-thegraph/contracts/token/GraphToken.sol::47 => mapping(address => bool) private _minters;
```

## [G-09] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, for example when used in for- and while-loops

The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas per loop

```
2022-10-thegraph/contracts/rewards/RewardsManager.sol::161 => for (uint256 i = 0; i < _subgraphDeploymentID.length; i++) {
2022-10-thegraph/contracts/staking/Staking.sol::924 => for (uint256 i = 0; i < _requests.length; i++) {
2022-10-thegraph/contracts/staking/Staking.sol::1050 => for (uint256 i = 0; i < _allocationID.length; i++) {
```

## [G-10] <x> += <y> costs more gas than <x> = <x> + <y> for state variables

use <x> = <x> + <y> or <x> = <x> - <y> instead to save gas

```
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::174 => r -= int256(0x0000000000000000000000000000001000000000000000000000000000000000); // - 32
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::181 => r -= int256(0x0000000000000000000000000000000800000000000000000000000000000000); // - 16
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::188 => r -= int256(0x0000000000000000000000000000000400000000000000000000000000000000); // - 8
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::195 => r -= int256(0x0000000000000000000000000000000200000000000000000000000000000000); // - 4
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::202 => r -= int256(0x0000000000000000000000000000000100000000000000000000000000000000); // - 2
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::209 => r -= int256(0x0000000000000000000000000000000080000000000000000000000000000000); // - 1
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::216 => r -= int256(0x0000000000000000000000000000000040000000000000000000000000000000); // - 0.5
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::223 => r -= int256(0x0000000000000000000000000000000020000000000000000000000000000000); // - 0.25
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::230 => r -= int256(0x0000000000000000000000000000000010000000000000000000000000000000); // - 0.125
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::240 => r += (z * (0x100000000000000000000000000000000 - y)) / 0x100000000000000000000000000000000;
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::242 => r += (z * (0x0aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa - y)) / 0x200000000000000000000000000000000;
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::244 => r += (z * (0x099999999999999999999999999999999 - y)) / 0x300000000000000000000000000000000;
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::246 => r += (z * (0x092492492492492492492492492492492 - y)) / 0x400000000000000000000000000000000;
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::248 => r += (z * (0x08e38e38e38e38e38e38e38e38e38e38e - y)) / 0x500000000000000000000000000000000;
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::250 => r += (z * (0x08ba2e8ba2e8ba2e8ba2e8ba2e8ba2e8b - y)) / 0x600000000000000000000000000000000;
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::252 => r += (z * (0x089d89d89d89d89d89d89d89d89d89d89 - y)) / 0x700000000000000000000000000000000;
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::254 => r += (z * (0x088888888888888888888888888888888 - y)) / 0x800000000000000000000000000000000; // add y^15 / 15 - y^16 / 16
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::282 => r += z * 0x10e1b3be415a0000; // add y^02 * (20! / 02!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::284 => r += z * 0x05a0913f6b1e0000; // add y^03 * (20! / 03!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::286 => r += z * 0x0168244fdac78000; // add y^04 * (20! / 04!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::288 => r += z * 0x004807432bc18000; // add y^05 * (20! / 05!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::290 => r += z * 0x000c0135dca04000; // add y^06 * (20! / 06!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::292 => r += z * 0x0001b707b1cdc000; // add y^07 * (20! / 07!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::294 => r += z * 0x000036e0f639b800; // add y^08 * (20! / 08!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::296 => r += z * 0x00000618fee9f800; // add y^09 * (20! / 09!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::298 => r += z * 0x0000009c197dcc00; // add y^10 * (20! / 10!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::300 => r += z * 0x0000000e30dce400; // add y^11 * (20! / 11!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::302 => r += z * 0x000000012ebd1300; // add y^12 * (20! / 12!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::304 => r += z * 0x0000000017499f00; // add y^13 * (20! / 13!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::306 => r += z * 0x0000000001a9d480; // add y^14 * (20! / 14!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::308 => r += z * 0x00000000001c6380; // add y^15 * (20! / 15!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::310 => r += z * 0x000000000001c638; // add y^16 * (20! / 16!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::312 => r += z * 0x0000000000001ab8; // add y^17 * (20! / 17!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::314 => r += z * 0x000000000000017c; // add y^18 * (20! / 18!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::316 => r += z * 0x0000000000000014; // add y^19 * (20! / 19!)
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::318 => r += z * 0x0000000000000001; // add y^20 * (20! / 20!)
2022-10-thegraph/contracts/staking/libs/Rebates.sol::73 => pool.unclaimedAllocationsCount += 1;
2022-10-thegraph/contracts/staking/libs/Rebates.sol::109 => pool.unclaimedAllocationsCount -= 1;
```

## [G-11] abi.encode() is less efficient than abi.encodePacked()

use abi.encodePacked() where possible to save gas

```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::249 => return abi.encode(seqNum);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::342 => abi.encode(emptyBytes, _data)
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::174 => return abi.encode(id);
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::275 => abi.encode(0, _data) // we don't need to track exitNums (b/c we have no fast exits) so we always use 0
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::88 => abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::162 => abi.encode(
2022-10-thegraph/contracts/token/GraphToken.sol::75 => abi.encode(
2022-10-thegraph/contracts/token/GraphToken.sol::110 => abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
```

## [G-12] Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

```
2022-10-thegraph/contracts/curation/Curation.sol::83 => require(_bondingCurve != address(0), "Bonding curve must be set");
2022-10-thegraph/contracts/curation/Curation.sol::109 => require(_defaultReserveRatio > 0, "Default reserve ratio must be > 0");
2022-10-thegraph/contracts/curation/Curation.sol::138 => require(_minimumCurationDeposit > 0, "Minimum curation deposit cannot be 0");
2022-10-thegraph/contracts/curation/Curation.sol::179 => require(_curationTokenMaster != address(0), "Token master must be non-empty");
2022-10-thegraph/contracts/curation/Curation.sol::180 => require(Address.isContract(_curationTokenMaster), "Token master must be a contract");
2022-10-thegraph/contracts/curation/Curation.sol::195 => require(msg.sender == address(staking()), "Caller must be the staking contract");
2022-10-thegraph/contracts/curation/Curation.sol::223 => require(_tokensIn > 0, "Cannot deposit zero tokens");
2022-10-thegraph/contracts/curation/Curation.sol::229 => require(signalOut >= _signalOutMin, "Slippage protection");
2022-10-thegraph/contracts/curation/Curation.sol::283 => require(_signalIn > 0, "Cannot burn zero signal");
2022-10-thegraph/contracts/curation/Curation.sol::293 => require(tokensOut >= _tokensOutMin, "Slippage protection");
2022-10-thegraph/contracts/epochs/EpochManager.sol::28 => require(_epochLength > 0, "Epoch length cannot be 0");
2022-10-thegraph/contracts/epochs/EpochManager.sol::47 => require(_epochLength > 0, "Epoch length cannot be 0");
2022-10-thegraph/contracts/epochs/EpochManager.sol::48 => require(_epochLength != epochLength, "Epoch length must be different to current");
2022-10-thegraph/contracts/epochs/EpochManager.sol::63 => require(!isCurrentEpochRun(), "Current epoch already run");
2022-10-thegraph/contracts/epochs/EpochManager.sol::95 => require(_block < currentBlock, "Can only retrieve past block hashes");
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::31 => require(_newPauseGuardian != address(0), "PauseGuardian must be set");
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::40 => require(!_paused, "Paused (contract)");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::74 => require(inbox != address(0), "INBOX_NOT_SET");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::78 => require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::82 => require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::110 => require(_inbox != address(0), "INVALID_INBOX");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::111 => require(_l1Router != address(0), "INVALID_L1_ROUTER");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::122 => require(_l2GRT != address(0), "INVALID_L2_GRT");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::132 => require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::142 => require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::153 => require(_newWhitelisted != address(0), "INVALID_ADDRESS");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::154 => require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::165 => require(_notWhitelisted != address(0), "INVALID_ADDRESS");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::166 => require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::200 => require(_l1Token == address(token), "TOKEN_NOT_GRT");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::201 => require(_amount > 0, "INVALID_ZERO_AMOUNT");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::202 => require(_to != address(0), "INVALID_DESTINATION");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::217 => require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::224 => require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::271 => require(_l1Token == address(token), "TOKEN_NOT_GRT");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::275 => require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
2022-10-thegraph/contracts/governance/Controller.sol::60 => require(_contractAddress != address(0), "Contract address must be set");
2022-10-thegraph/contracts/governance/Controller.sol::88 => require(_controller != address(0), "Controller must be set");
2022-10-thegraph/contracts/governance/Controller.sol::115 => require(_newPauseGuardian != address(0), "PauseGuardian must be set");
2022-10-thegraph/contracts/governance/Governed.sol::24 => require(msg.sender == governor, "Only Governor can call");
2022-10-thegraph/contracts/governance/Governed.sol::41 => require(_newGovernor != address(0), "Governor must be set");
2022-10-thegraph/contracts/governance/GraphGovernance.sol::39 => require(_governor != address(0), "governor != 0");
2022-10-thegraph/contracts/governance/GraphGovernance.sol::69 => require(_proposalId != 0x0, "!proposalId");
2022-10-thegraph/contracts/governance/GraphGovernance.sol::70 => require(_votes != 0x0, "!votes");
2022-10-thegraph/contracts/governance/GraphGovernance.sol::71 => require(_resolution != ProposalResolution.Null, "!resolved");
2022-10-thegraph/contracts/governance/GraphGovernance.sol::72 => require(!isProposalCreated(_proposalId), "proposed");
2022-10-thegraph/contracts/governance/GraphGovernance.sol::98 => require(_proposalId != 0x0, "!proposalId");
2022-10-thegraph/contracts/governance/GraphGovernance.sol::99 => require(_votes != 0x0, "!votes");
2022-10-thegraph/contracts/governance/GraphGovernance.sol::100 => require(_resolution != ProposalResolution.Null, "!resolved");
2022-10-thegraph/contracts/governance/GraphGovernance.sol::101 => require(isProposalCreated(_proposalId), "!proposed");
2022-10-thegraph/contracts/governance/Managed.sol::44 => require(!controller.paused(), "Paused");
2022-10-thegraph/contracts/governance/Managed.sol::45 => require(!controller.partialPaused(), "Partial-paused");
2022-10-thegraph/contracts/governance/Managed.sol::49 => require(!controller.paused(), "Paused");
2022-10-thegraph/contracts/governance/Managed.sol::53 => require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
2022-10-thegraph/contracts/governance/Managed.sol::57 => require(msg.sender == address(controller), "Caller must be Controller");
2022-10-thegraph/contracts/governance/Managed.sol::104 => require(_controller != address(0), "Controller must be set");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::98 => require(_l2Router != address(0), "INVALID_L2_ROUTER");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::108 => require(_l1GRT != address(0), "INVALID_L1_GRT");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::118 => require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::145 => require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::146 => require(_amount > 0, "INVALID_ZERO_AMOUNT");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::147 => require(msg.value == 0, "INVALID_NONZERO_VALUE");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::148 => require(_to != address(0), "INVALID_DESTINATION");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::153 => require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::233 => require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::234 => require(msg.value == 0, "INVALID_NONZERO_VALUE");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::60 => require(isMinter(msg.sender), "Only minter can call");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::94 => require(_owner == recoveredAddress, "GRT: invalid permit");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::95 => require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::106 => require(_account != address(0), "INVALID_MINTER");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::115 => require(_minters[_account], "NOT_A_MINTER");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::123 => require(_minters[msg.sender], "NOT_A_MINTER");
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::36 => require(msg.sender == gateway, "NOT_GATEWAY");
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::49 => require(_owner != address(0), "Owner must be set");
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::60 => require(_gw != address(0), "INVALID_GATEWAY");
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::70 => require(_addr != address(0), "INVALID_L1_ADDRESS");
2022-10-thegraph/contracts/rewards/RewardsManager.sol::95 => require(_issuanceRate >= MIN_ISSUANCE_RATE, "Issuance rate under minimum allowed");
2022-10-thegraph/contracts/rewards/RewardsManager.sol::160 => require(_subgraphDeploymentID.length == _deny.length, "!length");
2022-10-thegraph/contracts/rewards/RewardsManager.sol::411 => require(msg.sender == address(staking), "Caller must be the staking contract");
2022-10-thegraph/contracts/staking/Staking.sol::194 => require(slashers[msg.sender] == true, "!slasher");
2022-10-thegraph/contracts/staking/Staking.sol::254 => require(_minimumIndexerStake > 0, "!minimumIndexerStake");
2022-10-thegraph/contracts/staking/Staking.sol::272 => require(_thawingPeriod > 0, "!thawingPeriod");
2022-10-thegraph/contracts/staking/Staking.sol::291 => require(_percentage <= MAX_PPM, ">percentage");
2022-10-thegraph/contracts/staking/Staking.sol::310 => require(_percentage <= MAX_PPM, ">percentage");
2022-10-thegraph/contracts/staking/Staking.sol::328 => require(_channelDisputeEpochs > 0, "!channelDisputeEpochs");
2022-10-thegraph/contracts/staking/Staking.sol::369 => require(_alphaNumerator > 0 && _alphaDenominator > 0, "!alpha");
2022-10-thegraph/contracts/staking/Staking.sol::424 => require(_queryFeeCut <= MAX_PPM, ">queryFeeCut");
2022-10-thegraph/contracts/staking/Staking.sol::425 => require(_indexingRewardCut <= MAX_PPM, ">indexingRewardCut");
2022-10-thegraph/contracts/staking/Staking.sol::428 => require(_cooldownBlocks >= delegationParametersCooldown, "<cooldown");
2022-10-thegraph/contracts/staking/Staking.sol::486 => require(_delegationUnbondingPeriod > 0, "!delegationUnbondingPeriod");
2022-10-thegraph/contracts/staking/Staking.sol::505 => require(_percentage <= MAX_PPM, ">percentage");
2022-10-thegraph/contracts/staking/Staking.sol::516 => require(_slasher != address(0), "!slasher");
2022-10-thegraph/contracts/staking/Staking.sol::527 => require(_assetHolder != address(0), "!assetHolder");
2022-10-thegraph/contracts/staking/Staking.sol::666 => require(_operator != msg.sender, "operator == sender");
2022-10-thegraph/contracts/staking/Staking.sol::694 => require(_tokens > 0, "!tokens");
2022-10-thegraph/contracts/staking/Staking.sol::721 => require(indexerStake.tokensStaked > 0, "!stake");
2022-10-thegraph/contracts/staking/Staking.sol::725 => require(tokensToLock > 0, "!stake-avail");
2022-10-thegraph/contracts/staking/Staking.sol::729 => require(newStake == 0 || newStake >= minimumIndexerStake, "!minimumIndexerStake");
2022-10-thegraph/contracts/staking/Staking.sol::776 => require(_tokens > 0, "!tokens");
2022-10-thegraph/contracts/staking/Staking.sol::779 => require(_tokens >= _reward, "rewards>slash");
2022-10-thegraph/contracts/staking/Staking.sol::782 => require(indexerStake.tokensStaked > 0, "!stake");
2022-10-thegraph/contracts/staking/Staking.sol::783 => require(_tokens <= indexerStake.tokensStaked, "slash>stake");
2022-10-thegraph/contracts/staking/Staking.sol::786 => require(_beneficiary != address(0), "!beneficiary");
2022-10-thegraph/contracts/staking/Staking.sol::967 => require(_allocationID != address(0), "!alloc");
2022-10-thegraph/contracts/staking/Staking.sol::970 => require(assetHolders[msg.sender] == true, "!assetHolder");
2022-10-thegraph/contracts/staking/Staking.sol::974 => require(allocState != AllocationState.Null, "!collect");
2022-10-thegraph/contracts/staking/Staking.sol::1082 => require(tokensToWithdraw > 0, "!tokens");
2022-10-thegraph/contracts/staking/Staking.sol::1107 => require(_isAuth(_indexer), "!auth");
2022-10-thegraph/contracts/staking/Staking.sol::1110 => require(_allocationID != address(0), "!alloc");
2022-10-thegraph/contracts/staking/Staking.sol::1111 => require(_getAllocationState(_allocationID) == AllocationState.Null, "!null");
2022-10-thegraph/contracts/staking/Staking.sol::1117 => require(ECDSA.recover(digest, _proof) == _allocationID, "!proof");
2022-10-thegraph/contracts/staking/Staking.sol::1121 => require(getIndexerCapacity(_indexer) >= _tokens, "!capacity");
2022-10-thegraph/contracts/staking/Staking.sol::1177 => require(allocState == AllocationState.Active, "!active");
2022-10-thegraph/contracts/staking/Staking.sol::1185 => require(epochs > 0, "<epochs");
2022-10-thegraph/contracts/staking/Staking.sol::1193 => require(isIndexer, "!auth");
2022-10-thegraph/contracts/staking/Staking.sol::1259 => require(allocState == AllocationState.Finalized, "!finalized");
2022-10-thegraph/contracts/staking/Staking.sol::1323 => require(_tokens > 0, "!tokens");
2022-10-thegraph/contracts/staking/Staking.sol::1325 => require(_indexer != address(0), "!indexer");
2022-10-thegraph/contracts/staking/Staking.sol::1327 => require(stakes[_indexer].tokensStaked > 0, "!stake");
2022-10-thegraph/contracts/staking/Staking.sol::1341 => require(shares > 0, "!shares");
2022-10-thegraph/contracts/staking/Staking.sol::1368 => require(_shares > 0, "!shares");
2022-10-thegraph/contracts/staking/Staking.sol::1375 => require(delegation.shares >= _shares, "!shares-avail");
2022-10-thegraph/contracts/staking/Staking.sol::1422 => require(tokensToWithdraw > 0, "!tokens");
2022-10-thegraph/contracts/token/GraphToken.sol::56 => require(isMinter(msg.sender), "Only minter can call");
2022-10-thegraph/contracts/token/GraphToken.sol::117 => require(_owner == recoveredAddress, "GRT: invalid permit");
2022-10-thegraph/contracts/token/GraphToken.sol::118 => require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::105 => require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::141 => require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::157 => require(msg.sender != _admin(), "Cannot fallback to proxy target");
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::62 => require(msg.sender == _admin(), "Caller must be admin");
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::24 => require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::32 => require(msg.sender == _implementation(), "Caller must be the implementation");
```

## [G-13] Use a more recent version of solidity

Use a solidity version of at least 0.8.0 to get overflow protection without SafeMath
Use a solidity version of at least 0.8.2 to get compiler automatic inlining
Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads
Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

```
2022-10-thegraph/contracts/curation/Curation.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/curation/CurationStorage.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/curation/GraphCurationToken.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/curation/ICuration.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/curation/IGraphCurationToken.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/epochs/EpochManager.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/epochs/EpochManagerStorage.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/epochs/IEpochManager.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/gateway/ICallhookReceiver.sol::9 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/Controller.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/Governed.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/GraphGovernance.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/GraphGovernanceStorage.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/IController.sol::3 => pragma solidity >=0.6.12 <0.8.0;
2022-10-thegraph/contracts/governance/IGraphGovernance.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/IManaged.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/Managed.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/Pausable.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/rewards/IRewardsManager.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/rewards/RewardsManager.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/rewards/RewardsManagerStorage.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/staking/IStaking.sol::3 => pragma solidity >=0.6.12 <0.8.0;
2022-10-thegraph/contracts/staking/IStakingData.sol::3 => pragma solidity >=0.6.12 <0.8.0;
2022-10-thegraph/contracts/staking/Staking.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/staking/StakingStorage.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/staking/libs/Cobbs.sol::21 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::21 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/staking/libs/MathUtils.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/staking/libs/Rebates.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/staking/libs/Stakes.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/token/GraphToken.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/token/IGraphToken.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/IGraphProxy.sol::3 => pragma solidity ^0.7.6;
```

## [G-14] Prefix increments cheaper than Postfix increments

++i costs less gas than i++, especially when it's used in for-loops (--i/i-- too)
Saves 5 gas PER LOOP

```
2022-10-thegraph/contracts/rewards/RewardsManager.sol::161 => for (uint256 i = 0; i < _subgraphDeploymentID.length; i++) {
2022-10-thegraph/contracts/staking/Staking.sol::924 => for (uint256 i = 0; i < _requests.length; i++) {
2022-10-thegraph/contracts/staking/Staking.sol::1050 => for (uint256 i = 0; i < _allocationID.length; i++) {
```

## [G-15] Don't compare boolean expressions to boolean literals

The extran comparison wastes gas
if (<x> == true) => if (<x>), if (<x> == false) => if (!<x>)

```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::214 => extraData.length == 0 || callhookWhitelist[msg.sender] == true,
2022-10-thegraph/contracts/staking/Staking.sol::194 => require(slashers[msg.sender] == true, "!slasher");
2022-10-thegraph/contracts/staking/Staking.sol::202 => return msg.sender == _indexer || isOperator(msg.sender, _indexer) == true;
2022-10-thegraph/contracts/staking/Staking.sol::970 => require(assetHolders[msg.sender] == true, "!assetHolder");
```

## [G-16] Splitting require() statements that use && saves gas

Saves 16 gas per instance.
If you're using the Optimizer at 200, instead of using the && operator in a single require statement to check multiple conditions, multiple require statements with 1 condition per require statement should be used to save gas:

```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::142 => require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
2022-10-thegraph/contracts/staking/Staking.sol::369 => require(_alphaNumerator > 0 && _alphaDenominator > 0, "!alpha");
```

## [G-17] Usage of assert() instead of require()

Between solc 0.4.10 and 0.8.0, require() used REVERT (0xfd) opcode which refunded remaining gas on failure while assert() used INVALID (0xfe) opcode which consumed all the supplied gas.
require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking (properly functioning code should never reach a failing assert statement, unless there is a bug in your contract you should fix).

```
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::47 => assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::48 => assert(
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::51 => assert(
```

## [G-18] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public and can save gas by doing so.

```
2022-10-thegraph/contracts/curation/GraphCurationToken.sol::45 => function burnFrom(address _account, uint256 _amount) public onlyGovernor {
2022-10-thegraph/contracts/governance/Controller.sol::78 => function getContractProxy(bytes32 _id) public view override returns (address) {
2022-10-thegraph/contracts/staking/Staking.sol::613 => function isDelegator(address _indexer, address _delegator) public view override returns (bool) {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::30 => function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::43 => function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::55 => function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::68 => function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::77 => function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {
```

## [G-19] Not using the named return variables when a function returns, wastes deployment gas

It is not necessary to have both a named return and a return statement.

```
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::286 => function parseOutboundData(bytes memory _data) private view returns (address, bytes memory) {
2022-10-thegraph/contracts/staking/libs/Cobbs.sol::51 => ) public pure returns (uint256 rewards) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::146 => function toInteger(int256 f) internal pure returns (int256 n) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::151 => function ln(int256 x) internal pure returns (int256 r) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::258 => function exp(int256 x) internal pure returns (int256 r) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::380 => function _mul(int256 a, int256 b) private pure returns (int256 c) {
```

## [G-20] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

```
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::51 => mapping(address => bool) private _minters;
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::52 => mapping(address => uint256) public nonces;
2022-10-thegraph/contracts/staking/StakingStorage.sol::39 => mapping(address => Stakes.Indexer) public stakes;
2022-10-thegraph/contracts/staking/StakingStorage.sol::42 => mapping(address => IStakingData.Allocation) public allocations;
2022-10-thegraph/contracts/staking/StakingStorage.sol::53 => mapping(address => bool) public slashers;
2022-10-thegraph/contracts/staking/StakingStorage.sol::73 => mapping(address => IStakingData.DelegationPool) public delegationPools;
2022-10-thegraph/contracts/staking/StakingStorage.sol::78 => mapping(address => mapping(address => bool)) public operatorAuth;
2022-10-thegraph/contracts/staking/StakingStorage.sol::83 => mapping(address => bool) public assetHolders;
2022-10-thegraph/contracts/staking/StakingStorage.sol::88 => mapping(address => address) public rewardsDestination;
2022-10-thegraph/contracts/token/GraphToken.sol::47 => mapping(address => bool) private _minters;
2022-10-thegraph/contracts/token/GraphToken.sol::48 => mapping(address => uint256) public nonces;
```

## [G-21] Use assembly to check for address(0)

Saves 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
 if iszero(_addr) {
  mstore(0x00, "zero address")
  revert(0x00, 0x20)
 }
}
```

instances:

```
2022-10-thegraph/contracts/curation/Curation.sol::83 => require(_bondingCurve != address(0), "Bonding curve must be set");
2022-10-thegraph/contracts/curation/Curation.sol::179 => require(_curationTokenMaster != address(0), "Token master must be non-empty");
2022-10-thegraph/contracts/curation/Curation.sol::468 => if (address(rewardsManager) != address(0)) {
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::31 => require(_newPauseGuardian != address(0), "PauseGuardian must be set");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::74 => require(inbox != address(0), "INBOX_NOT_SET");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::110 => require(_inbox != address(0), "INVALID_INBOX");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::111 => require(_l1Router != address(0), "INVALID_L1_ROUTER");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::122 => require(_l2GRT != address(0), "INVALID_L2_GRT");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::132 => require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::142 => require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::153 => require(_newWhitelisted != address(0), "INVALID_ADDRESS");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::165 => require(_notWhitelisted != address(0), "INVALID_ADDRESS");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::202 => require(_to != address(0), "INVALID_DESTINATION");
2022-10-thegraph/contracts/governance/Controller.sol::60 => require(_contractAddress != address(0), "Contract address must be set");
2022-10-thegraph/contracts/governance/Controller.sol::88 => require(_controller != address(0), "Controller must be set");
2022-10-thegraph/contracts/governance/Controller.sol::115 => require(_newPauseGuardian != address(0), "PauseGuardian must be set");
2022-10-thegraph/contracts/governance/Governed.sol::41 => require(_newGovernor != address(0), "Governor must be set");
2022-10-thegraph/contracts/governance/Governed.sol::55 => pendingGovernor != address(0) && msg.sender == pendingGovernor,
2022-10-thegraph/contracts/governance/GraphGovernance.sol::39 => require(_governor != address(0), "governor != 0");
2022-10-thegraph/contracts/governance/Managed.sol::104 => require(_controller != address(0), "Controller must be set");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::98 => require(_l2Router != address(0), "INVALID_L2_ROUTER");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::108 => require(_l1GRT != address(0), "INVALID_L1_GRT");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::118 => require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::148 => require(_to != address(0), "INVALID_DESTINATION");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::106 => require(_account != address(0), "INVALID_MINTER");
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::49 => require(_owner != address(0), "Owner must be set");
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::60 => require(_gw != address(0), "INVALID_GATEWAY");
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::70 => require(_addr != address(0), "INVALID_L1_ADDRESS");
2022-10-thegraph/contracts/staking/Staking.sol::516 => require(_slasher != address(0), "!slasher");
2022-10-thegraph/contracts/staking/Staking.sol::527 => require(_assetHolder != address(0), "!assetHolder");
2022-10-thegraph/contracts/staking/Staking.sol::786 => require(_beneficiary != address(0), "!beneficiary");
2022-10-thegraph/contracts/staking/Staking.sol::967 => require(_allocationID != address(0), "!alloc");
2022-10-thegraph/contracts/staking/Staking.sol::1110 => require(_allocationID != address(0), "!alloc");
2022-10-thegraph/contracts/staking/Staking.sol::1325 => require(_indexer != address(0), "!indexer");
2022-10-thegraph/contracts/staking/Staking.sol::1432 => if (_delegateToIndexer != address(0)) {
2022-10-thegraph/contracts/staking/Staking.sol::1505 => bool isCurationEnabled = _curationPercentage > 0 && address(curation) != address(0);
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::105 => require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::143 => _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
```

## [G-22] internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```
2022-10-thegraph/contracts/governance/Governed.sol::31 => function _initialize(address _initGovernor) internal {
2022-10-thegraph/contracts/governance/Managed.sol::43 => function _notPartialPaused() internal view {
2022-10-thegraph/contracts/governance/Managed.sol::48 => function _notPaused() internal view virtual {
2022-10-thegraph/contracts/governance/Managed.sol::52 => function _onlyGovernor() internal view {
2022-10-thegraph/contracts/governance/Managed.sol::56 => function _onlyController() internal view {
2022-10-thegraph/contracts/governance/Managed.sol::87 => function _initialize(address _controller) internal {
2022-10-thegraph/contracts/governance/Managed.sol::113 => function curation() internal view returns (ICuration) {
2022-10-thegraph/contracts/governance/Managed.sol::121 => function epochManager() internal view returns (IEpochManager) {
2022-10-thegraph/contracts/governance/Managed.sol::129 => function rewardsManager() internal view returns (IRewardsManager) {
2022-10-thegraph/contracts/governance/Managed.sol::137 => function staking() internal view returns (IStaking) {
2022-10-thegraph/contracts/governance/Managed.sol::145 => function graphToken() internal view returns (IGraphToken) {
2022-10-thegraph/contracts/governance/Managed.sol::153 => function graphTokenGateway() internal view returns (ITokenGateway) {
2022-10-thegraph/contracts/governance/Pausable.sol::26 => function _setPartialPaused(bool _toPause) internal {
2022-10-thegraph/contracts/governance/Pausable.sol::40 => function _setPaused(bool _toPause) internal {
2022-10-thegraph/contracts/governance/Pausable.sol::55 => function _setPauseGuardian(address newPauseGuardian) internal {
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::150 => function _initialize(address _owner, uint256 _initialSupply) internal {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::47 => function one() internal pure returns (int256 f) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::57 => function sub(int256 a, int256 b) internal pure returns (int256 c) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::86 => function uintMul(int256 f, uint256 u) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::98 => function abs(int256 f) internal pure returns (int256 c) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::110 => function invert(int256 f) internal pure returns (int256 c) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::146 => function toInteger(int256 f) internal pure returns (int256 n) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::151 => function ln(int256 x) internal pure returns (int256 r) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::258 => function exp(int256 x) internal pure returns (int256 r) {
2022-10-thegraph/contracts/staking/libs/MathUtils.sol::35 => function min(uint256 x, uint256 y) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/MathUtils.sol::42 => function diffOrZero(uint256 x, uint256 y) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Rebates.sol::48 => function exists(Rebates.Pool storage pool) internal view returns (bool) {
2022-10-thegraph/contracts/staking/libs/Rebates.sol::55 => function unclaimedFees(Rebates.Pool storage pool) internal view returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::31 => function deposit(Stakes.Indexer storage stake, uint256 _tokens) internal {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::40 => function release(Stakes.Indexer storage stake, uint256 _tokens) internal {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::49 => function allocate(Stakes.Indexer storage stake, uint256 _tokens) internal {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::58 => function unallocate(Stakes.Indexer storage stake, uint256 _tokens) internal {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::94 => function unlockTokens(Stakes.Indexer storage stake, uint256 _tokens) internal {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::106 => function withdrawTokens(Stakes.Indexer storage stake) internal returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::126 => function tokensUsed(Stakes.Indexer memory stake) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::137 => function tokensSecureStake(Stakes.Indexer memory stake) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::149 => function tokensAvailable(Stakes.Indexer memory stake) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::189 => function tokensWithdrawable(Stakes.Indexer memory stake) internal view returns (uint256) {
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::153 => * This function does not return to its internal call site, it will return directly to the
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::80 => function _setAdmin(address _newAdmin) internal {
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::93 => function _implementation() internal view returns (address impl) {
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::104 => function _pendingImplementation() internal view returns (address impl) {
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::115 => function _setImplementation(address _newImplementation) internal {
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::130 => function _setPendingImplementation(address _newImplementation) internal {
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::40 => function _implementation() internal view returns (address impl) {
```

## [G-23] internal functions not called by the contract should be removed to save deployment gas

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword

```
2022-10-thegraph/contracts/governance/Governed.sol::31 => function _initialize(address _initGovernor) internal {
2022-10-thegraph/contracts/governance/Managed.sol::87 => function _initialize(address _controller) internal {
2022-10-thegraph/contracts/governance/Managed.sol::113 => function curation() internal view returns (ICuration) {
2022-10-thegraph/contracts/governance/Managed.sol::121 => function epochManager() internal view returns (IEpochManager) {
2022-10-thegraph/contracts/governance/Managed.sol::129 => function rewardsManager() internal view returns (IRewardsManager) {
2022-10-thegraph/contracts/governance/Managed.sol::137 => function staking() internal view returns (IStaking) {
2022-10-thegraph/contracts/governance/Managed.sol::145 => function graphToken() internal view returns (IGraphToken) {
2022-10-thegraph/contracts/governance/Managed.sol::153 => function graphTokenGateway() internal view returns (ITokenGateway) {
2022-10-thegraph/contracts/governance/Pausable.sol::26 => function _setPartialPaused(bool _toPause) internal {
2022-10-thegraph/contracts/governance/Pausable.sol::40 => function _setPaused(bool _toPause) internal {
2022-10-thegraph/contracts/governance/Pausable.sol::55 => function _setPauseGuardian(address newPauseGuardian) internal {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::47 => function one() internal pure returns (int256 f) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::57 => function sub(int256 a, int256 b) internal pure returns (int256 c) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::86 => function uintMul(int256 f, uint256 u) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::98 => function abs(int256 f) internal pure returns (int256 c) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::110 => function invert(int256 f) internal pure returns (int256 c) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::146 => function toInteger(int256 f) internal pure returns (int256 n) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::151 => function ln(int256 x) internal pure returns (int256 r) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::258 => function exp(int256 x) internal pure returns (int256 r) {
2022-10-thegraph/contracts/staking/libs/MathUtils.sol::35 => function min(uint256 x, uint256 y) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/MathUtils.sol::42 => function diffOrZero(uint256 x, uint256 y) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Rebates.sol::48 => function exists(Rebates.Pool storage pool) internal view returns (bool) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::31 => function deposit(Stakes.Indexer storage stake, uint256 _tokens) internal {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::58 => function unallocate(Stakes.Indexer storage stake, uint256 _tokens) internal {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::106 => function withdrawTokens(Stakes.Indexer storage stake) internal returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::137 => function tokensSecureStake(Stakes.Indexer memory stake) internal pure returns (uint256) {
2022-10-thegraph/contracts/staking/libs/Stakes.sol::149 => function tokensAvailable(Stakes.Indexer memory stake) internal pure returns (uint256) {
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::153 => * This function does not return to its internal call site, it will return directly to the
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::80 => function _setAdmin(address _newAdmin) internal {
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::115 => function _setImplementation(address _newImplementation) internal {
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::130 => function _setPendingImplementation(address _newImplementation) internal {
```