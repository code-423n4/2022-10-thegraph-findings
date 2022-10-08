

### [G01] State variables only set in the constructor should be declared `immutable`

#### Impact
Avoids a Gsset (20000 gas)
#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::25 => address public l2GRT;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::27 => address public inbox;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::29 => address public l1Router;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::31 => address public l2Counterpart;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::33 => address public escrow;
2022-10-thegraph/contracts/governance/Governed.sol::12 => address public governor;
2022-10-thegraph/contracts/governance/Governed.sol::13 => address public pendingGovernor;
2022-10-thegraph/contracts/governance/Managed.sol::27 => IController public controller;
2022-10-thegraph/contracts/governance/Pausable.sol::13 => uint256 public lastPausePartialTime;
2022-10-thegraph/contracts/governance/Pausable.sol::14 => uint256 public lastPauseTime;
2022-10-thegraph/contracts/governance/Pausable.sol::17 => address public pauseGuardian;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::27 => address public l1GRT;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::29 => address public l1Counterpart;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::31 => address public l2Router;
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::19 => address public gateway;
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::21 => address public override l1Address;
```



### [G02] State variables can be packed into fewer storage slots

#### Impact
If variables occupying the same slot are both written the same 
function or by the constructor, avoids a separate Gsset (20000 gas). 
Reads of the variables are also cheaper
#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::25 => address public l2GRT;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::27 => address public inbox;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::29 => address public l1Router;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::31 => address public l2Counterpart;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::33 => address public escrow;
2022-10-thegraph/contracts/governance/Governed.sol::12 => address public governor;
2022-10-thegraph/contracts/governance/Governed.sol::13 => address public pendingGovernor;
2022-10-thegraph/contracts/governance/Pausable.sol::17 => address public pauseGuardian;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::27 => address public l1GRT;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::29 => address public l1Counterpart;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::31 => address public l2Router;
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::19 => address public gateway;
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::21 => address public override l1Address;
```





### [G03] `require()`/`revert()` strings longer than 32 bytes cost extra gas



#### Findings:
```
contracts/governance/Managed.sol::53 => require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::105 => require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::141 => require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::32 => require(msg.sender == _implementation(), "Caller must be the implementation");
```



### [G04] Not using the named return variables when a function returns, wastes deployment gas



#### Findings:
```
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::55 => return _paused;
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::142 => return _minters[_account];
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::70 => return _admin();
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::83 => return _implementation();
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::96 => return _pendingImplementation();
```



### [G05] Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement



#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::201 => require(_amount > 0, "INVALID_ZERO_AMOUNT");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::217 => require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::146 => require(_amount > 0, "INVALID_ZERO_AMOUNT");
```






### [G06] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

#### Impact
> When using elements that are smaller than 32 bytes, your 
contract’s gas usage may be higher. This is because the EVM operates on 
32 bytes at a time. Therefore, if the element is smaller than that, the 
EVM must use more operations in order to reduce the size of the element 
from 32 bytes to the desired size.
> 

[https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html](https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html)
Use a larger size then downcast where needed
#### Findings:
```
2022-10-thegraph/contracts/curation/ICuration.sol::10 => function setDefaultReserveRatio(uint32 _defaultReserveRatio) external;
2022-10-thegraph/contracts/curation/ICuration.sol::14 => function setCurationTaxPercentage(uint32 _percentage) external;
2022-10-thegraph/contracts/curation/ICuration.sol::57 => function curationTaxPercentage() external view returns (uint32);
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::79 => uint8 _v,
2022-10-thegraph/contracts/staking/IStakingData.sol::37 => uint32 cooldownBlocks; // Blocks to wait before updating parameters
2022-10-thegraph/contracts/staking/IStakingData.sol::38 => uint32 indexingRewardCut; // in PPM
2022-10-thegraph/contracts/staking/IStakingData.sol::39 => uint32 queryFeeCut; // in PPM
2022-10-thegraph/contracts/staking/Staking.sol::32 => function setThawingPeriod(uint32 _thawingPeriod) external;
2022-10-thegraph/contracts/staking/Staking.sol::34 => function setCurationPercentage(uint32 _percentage) external;
2022-10-thegraph/contracts/staking/Staking.sol::36 => function setProtocolPercentage(uint32 _percentage) external;
2022-10-thegraph/contracts/staking/Staking.sol::38 => function setChannelDisputeEpochs(uint32 _channelDisputeEpochs) external;
2022-10-thegraph/contracts/staking/Staking.sol::40 => function setMaxAllocationEpochs(uint32 _maxAllocationEpochs) external;
2022-10-thegraph/contracts/staking/Staking.sol::42 => function setRebateRatio(uint32 _alphaNumerator, uint32 _alphaDenominator) external;
2022-10-thegraph/contracts/staking/Staking.sol::44 => function setDelegationRatio(uint32 _delegationRatio) external;
2022-10-thegraph/contracts/staking/Staking.sol::47 => uint32 _indexingRewardCut,
2022-10-thegraph/contracts/staking/Staking.sol::48 => uint32 _queryFeeCut,
2022-10-thegraph/contracts/staking/Staking.sol::49 => uint32 _cooldownBlocks
2022-10-thegraph/contracts/staking/Staking.sol::52 => function setDelegationParametersCooldown(uint32 _blocks) external;
2022-10-thegraph/contracts/staking/Staking.sol::54 => function setDelegationUnbondingPeriod(uint32 _delegationUnbondingPeriod) external;
2022-10-thegraph/contracts/staking/Staking.sol::56 => function setDelegationTaxPercentage(uint32 _percentage) external;
```



### [G07] Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

#### Impact
See [this](https://github.com/ethereum/solidity/issues/9232) issue for a detail description of the issue
#### Findings:
```
2022-10-thegraph/contracts/governance/Managed.sol::174 => bytes32 nameHash = keccak256(abi.encodePacked(_name));
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::38 => bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::39 => bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::83 => bytes32 digest = keccak256(
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::53 => bytes32(uint256(keccak256("eip1967.proxy.pendingImplementation")) - 1)
```



### [G08] Duplicated `require()`/`revert()` checks should be refactored to a modifier or function



#### Findings:
```

2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::122 => require(_l2GRT != address(0), "INVALID_L2_GRT");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::200 => require(_l1Token == address(token), "TOKEN_NOT_GRT");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::271 => require(_l1Token == address(token), "TOKEN_NOT_GRT");


2022-10-thegraph/contracts/governance/Managed.sol::44 => require(!controller.paused(), "Paused");
2022-10-thegraph/contracts/governance/Managed.sol::49 => require(!controller.paused(), "Paused");

2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::145 => require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::147 => require(msg.value == 0, "INVALID_NONZERO_VALUE");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::153 => require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::233 => require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::234 => require(msg.value == 0, "INVALID_NONZERO_VALUE");


2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::106 => require(_account != address(0), "INVALID_MINTER");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::115 => require(_minters[_account], "NOT_A_MINTER");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::123 => require(_minters[msg.sender], "NOT_A_MINTER");


2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::34 => require(success);
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::47 => require(success);
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::59 => require(success);
```



### [G09] `require()` or `revert()` statements that check input arguments should be at the top of the function

#### Impact
Checks that involve constants should come before checks that involve state variables
#### Findings:
```
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
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::202 => require(_to != address(0), "INVALID_DESTINATION");
2022-10-thegraph/contracts/governance/Governed.sol::41 => require(_newGovernor != address(0), "Governor must be set");
2022-10-thegraph/contracts/governance/Managed.sol::104 => require(_controller != address(0), "Controller must be set");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::69 => require(
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::98 => require(_l2Router != address(0), "INVALID_L2_ROUTER");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::108 => require(_l1GRT != address(0), "INVALID_L1_GRT");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::118 => require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::148 => require(_to != address(0), "INVALID_DESTINATION");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::106 => require(_account != address(0), "INVALID_MINTER");
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::49 => require(_owner != address(0), "Owner must be set");
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::60 => require(_gw != address(0), "INVALID_GATEWAY");
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::70 => require(_addr != address(0), "INVALID_L1_ADDRESS");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::105 => require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```



### [G10] Use custom errors rather than `revert()`/`require()` strings to save deployment gas


#### Findings:
```
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
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::213 => require(
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::217 => require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::224 => require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::271 => require(_l1Token == address(token), "TOKEN_NOT_GRT");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::275 => require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
2022-10-thegraph/contracts/governance/Governed.sol::24 => require(msg.sender == governor, "Only Governor can call");
2022-10-thegraph/contracts/governance/Governed.sol::41 => require(_newGovernor != address(0), "Governor must be set");
2022-10-thegraph/contracts/governance/Governed.sol::54 => require(
2022-10-thegraph/contracts/governance/Managed.sol::44 => require(!controller.paused(), "Paused");
2022-10-thegraph/contracts/governance/Managed.sol::45 => require(!controller.partialPaused(), "Partial-paused");
2022-10-thegraph/contracts/governance/Managed.sol::49 => require(!controller.paused(), "Paused");
2022-10-thegraph/contracts/governance/Managed.sol::53 => require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
2022-10-thegraph/contracts/governance/Managed.sol::57 => require(msg.sender == address(controller), "Caller must be Controller");
2022-10-thegraph/contracts/governance/Managed.sol::104 => require(_controller != address(0), "Controller must be set");
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::69 => require(
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
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::105 => require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::133 => require(success);
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::141 => require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::142 => require(
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::157 => require(msg.sender != _admin(), "Cannot fallback to proxy target");
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::62 => require(msg.sender == _admin(), "Caller must be admin");
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::24 => require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::32 => require(msg.sender == _implementation(), "Caller must be the implementation");
```



### [G11] Functions guaranteed to revert when called by normal users can be marked `payable`

#### Impact
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
#### Findings:
```
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
2022-10-thegraph/contracts/governance/Governed.sol::40 => function transferOwnership(address _newGovernor) external onlyGovernor {
2022-10-thegraph/contracts/governance/Managed.sol::52 => function _onlyGovernor() internal view {
2022-10-thegraph/contracts/governance/Managed.sol::56 => function _onlyController() internal view {
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
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::68 => function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::77 => function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::86 => function acceptProxy(GraphUpgradeable _implementation, IGraphProxy _proxy) public onlyGovernor {
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::50 => function acceptProxy(IGraphProxy _proxy) external onlyProxyAdmin(_proxy) {
```



### [G12] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.13 to have external calls skip
 contract existence checks if the external call has a return value
#### Findings:
```
2022-10-thegraph/contracts/curation/ICuration.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/curation/IGraphCurationToken.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/epochs/IEpochManager.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/gateway/ICallhookReceiver.sol::9 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/Governed.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/IController.sol::3 => pragma solidity >=0.6.12 <0.8.0;
2022-10-thegraph/contracts/governance/Managed.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/Pausable.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/rewards/IRewardsManager.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/staking/IStakingData.sol::3 => pragma solidity >=0.6.12 <0.8.0;
2022-10-thegraph/contracts/staking/Staking.sol::3 => pragma solidity >=0.6.12 <0.8.0;
2022-10-thegraph/contracts/token/IGraphToken.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/IGraphProxy.sol::3 => pragma solidity ^0.7.6;
```


### [G13] Use `calldata` instead of `memory` for function parameters

#### Impact
Use calldata instead of memory for function parameters. Having function arguments use calldata instead of memory can save gas.
#### Findings:
```

2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::290 => function parseOutboundData(bytes memory _data)
2022-10-thegraph/contracts/governance/Managed.sol::173 => function _syncContract(string memory _name) internal {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::286 => function parseOutboundData(bytes memory _data) private view returns (address, bytes memory) {
2022-10-thegraph/contracts/staking/Staking.sol::143 => function getAllocation(address _allocationID) external view returns (Allocation memory);
```



### [G14] Caching external values in memory


#### Findings:
```

2022-10-thegraph/contracts/governance/Managed.sol::44 => require(!controller.paused(), "Paused");
2022-10-thegraph/contracts/governance/Managed.sol::45 => require(!controller.partialPaused(), "Partial-paused");
2022-10-thegraph/contracts/governance/Managed.sol::49 => require(!controller.paused(), "Paused");
2022-10-thegraph/contracts/governance/Managed.sol::53 => require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
2022-10-thegraph/contracts/governance/Managed.sol::57 => require(msg.sender == address(controller), "Caller must be Controller");
2022-10-thegraph/contracts/governance/Managed.sol::104 => require(_controller != address(0), "Controller must be set");
```


### [G15] Amounts should be checked for 0 before calling a transfer

#### Impact
Checking non-zero transfer values can avoid an expensive external call and save gas.

While this is done at some places, it’s not consistently done in the solution.

I suggest adding a non-zero-value check here
#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::235 => token.transferFrom(from, escrow, _amount);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::276 => token.transferFrom(escrow, _to, _amount);
```






### [G16] Multiple `address` mappings can be combined into a single `mapping` of an `address` to a `struct`, where appropriate

#### Impact
Saves a storage slot for the mapping. Depending on the circumstances 
and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. 
Reads and subsequent writes can also be cheaper when a function requires
 both values and they both fit in the same storage slot
#### Findings:
```

2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::51 => mapping(address => bool) private _minters;
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::52 => mapping(address => uint256) public nonces;
```


### [G17] Using `bools` for storage incurs overhead


#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::35 => mapping(address => bool) public callhookWhitelist;
2022-10-thegraph/contracts/governance/Pausable.sol::8 => bool internal _partialPaused;
2022-10-thegraph/contracts/governance/Pausable.sol::10 => bool internal _paused;
2022-10-thegraph/contracts/governance/Pausable.sol::19 => event PartialPauseChanged(bool isPaused);
2022-10-thegraph/contracts/governance/Pausable.sol::20 => event PauseChanged(bool isPaused);
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::51 => mapping(address => bool) private _minters;
```


### [G18] Usage of `assert()` instead of `require()`

#### Impact
Between solc 0.4.10 and 0.8.0, require() used REVERT (0xfd) opcode which refunded remaining gas on failure while assert() used INVALID (0xfe) opcode which consumed all the supplied gas. (see https://docs.soliditylang.org/en/v0.8.1/control-structures.html#error-handling-assert-require-revert-and-exceptions).require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking (properly functioning code should never reach a failing assert statement, unless there is a bug in your contract you should fix).
From the current usage of assert, my guess is that they can be replaced with require, unless a Panic really is intended.
#### Findings:
```
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::47 => assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::48 => assert(
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::51 => assert(
```




### [G19] `abi.encode()` is less efficient than abi.encodePacked()


#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::249 => return abi.encode(seqNum);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::342 => abi.encode(emptyBytes, _data)
2022-10-thegraph/contracts/governance/Managed.sol::174 => bytes32 nameHash = keccak256(abi.encodePacked(_name));
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::174 => return abi.encode(id);
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::275 => abi.encode(0, _data) // we don't need to track exitNums (b/c we have no fast exits) so we always use 0
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::88 => abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::162 => abi.encode(
```



### [G20] Don’t compare boolean expressions to boolean literals

#### Impact
if (<x> == true) => if (<x>), if (<x> == false) => if (!<x>)
#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::214 => extraData.length == 0 || callhookWhitelist[msg.sender] == true,
```



### [G21] Optimize names to save gas

#### Impact
public/external function names and public member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9)
 link for an example of how it works. Below are the interfaces/abstract 
contracts that can be optimized so that the most frequently-called 
functions use the least amount of gas possible during method lookup. 
Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, per sorted position shifted
#### Findings:
```
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::14 => abstract contract GraphTokenGateway is GraphUpgradeable, Pausable, Managed, ITokenGateway {
```




