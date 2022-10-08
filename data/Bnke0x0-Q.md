
### [L01] require() should be used instead of assert()



#### Findings:
```
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::47 => assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::48 => assert(
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::51 => assert(
```




### [L02] Missing checks for `address(0x0)` when assigning values to `address` state variables


#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::101 => _paused = true;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::111 => require(_l1Router != address(0), "INVALID_L1_ROUTER");
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::112 => inbox = _inbox;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::113 => l1Router = _l1Router;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::123 => l2GRT = _l2GRT;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::133 => l2Counterpart = _l2Counterpart;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::143 => escrow = _escrow;
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::307 => extraData = _data;
2022-10-thegraph/contracts/governance/Governed.sol::32 => governor = _initGovernor;
2022-10-thegraph/contracts/governance/Governed.sol::44 => pendingGovernor = _newGovernor;
2022-10-thegraph/contracts/governance/Pausable.sol::44 => _paused = _toPause;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::89 => _paused = true;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::99 => l2Router = _l2Router;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::109 => l1GRT = _l1GRT;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::119 => l1Counterpart = _l1Counterpart;
```


### [L03] `initialize` functions can be front-run

#### Impact
See [this](https://github.com/code-423n4/2021-10-badgerdao-findings/issues/40) finding from a prior badger-dao contest for details
#### Findings:
```
2022-10-thegraph/contracts/curation/IGraphCurationToken.sol::8 => function initialize(address _owner) external;
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::20 => function initialize(address _controller) external onlyImpl {
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::99 => function initialize(address _controller) external onlyImpl {
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::100 => Managed._initialize(_controller);
2022-10-thegraph/contracts/governance/Governed.sol::31 => function _initialize(address _initGovernor) internal {
2022-10-thegraph/contracts/governance/Managed.sol::87 => function _initialize(address _controller) internal {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::87 => function initialize(address _controller) external onlyImpl {
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::150 => function _initialize(address _owner, uint256 _initialSupply) internal {
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::48 => function initialize(address _owner) external onlyImpl {
```


### [L04] Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions

#### Impact
See [this](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps)
 link for a description of this storage variable. While some contracts 
may not currently be sub-classed, adding the variable now protects 
against forgetting to add it in the future.
#### Findings:
```
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::15 => contract BridgeEscrow is GraphUpgradeable, Managed {
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::14 => abstract contract GraphTokenGateway is GraphUpgradeable, Pausable, Managed, ITokenGateway {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::23 => contract L2GraphTokenGateway is GraphTokenGateway, L2ArbitrumMessenger, ReentrancyGuardUpgradeable {
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::28 => contract GraphTokenUpgradeable is GraphUpgradeable, Governed, ERC20BurnableUpgradeable {
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::15 => contract L2GraphToken is GraphTokenUpgradeable, IArbToken {
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::11 => contract GraphUpgradeable {
```




### [L05] `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`

#### Impact
Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). “Unless there is a compelling reason, abi.encode should be preferred”. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.
#### Findings:
```
2022-10-thegraph/contracts/governance/Managed.sol::174 => bytes32 nameHash = keccak256(abi.encodePacked(_name));
```


### [L06] approve should be replaced with safeApprove or safeIncreaseAllowance() / safeDecreaseAllowance()

#### Impact
approve is subject to a known front-running attack. Consider using safeApprove instead:
#### Findings:
```
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::29 => graphToken().approve(_spender, type(uint256).max);
```



### [L07] `_safeMint()` should be used rather than `_mint()` wherever possible

#### Impact
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### Findings:
```
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::133 => _mint(_to, _amount);
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::155 => _mint(_owner, _initialSupply);
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::81 => _mint(_account, _amount);
```


### [L08] Return values of `transfer()`/`transferFrom()` not checked

#### Impact
Not all IERC20 implementations revert() when there’s a failure in transfer()/transferFrom(). The function signature has a boolean
 return value and they indicate errors that way instead. By not checking
 the return value, operations that should have been marked as failed may 
potentially go through without actually making a payment
#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::235 => token.transferFrom(from, escrow, _amount);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::276 => token.transferFrom(escrow, _to, _amount);
```



### [L09] FeeOnTransfer Tokens not Supported

#### Impact
There exist ERC20 tokens that charge a fee for every `transfer()` or `transferFrom()`.

If these tokens are unsupported, ensure there is proper documentation about them.
#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::235 => token.transferFrom(from, escrow, _amount);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::276 => token.transferFrom(escrow, _to, _amount);
```


### [L10] Unspecific Compiler Version Pragma

#### Impact
Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.
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
2022-10-thegraph/contracts/governance/Managed.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/Pausable.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/rewards/IRewardsManager.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/token/IGraphToken.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/IGraphProxy.sol::3 => pragma solidity ^0.7.6;
```




#### Non-Critical Issues

### [N01] Adding a return statement when the function defines a named return variable, is redundant


#### Findings:
```
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::55 => return _paused;
2022-10-thegraph/contracts/governance/Managed.sol::166 => return contractAddress;
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::70 => return _admin();
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::83 => return _implementation();
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::96 => return _pendingImplementation();
```


### [N02] `require()`/`revert()` statements should have descriptive reason strings



#### Findings:
```
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::133 => require(success);
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::179 => revert(ptr, size)
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::34 => require(success);
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::47 => require(success);
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::59 => require(success);
```


### [N03] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.12 to get string.concat() to be used instead of abi.encodePacked(<str>,<str>)
#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/governance/Managed.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::3 => pragma solidity ^0.7.6;
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::3 => pragma solidity ^0.7.6;
```


### [N04] Variable names that consist of all capital letters should be reserved for `const`/`immutable `variables

#### Impact
If the variable needs to be different based on which class it comes from, a view/pure function should be used instead (e.g. like [this](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/76eee35971c2541585e05cbf258510dda7b2fbc6/contracts/token/ERC20/extensions/draft-IERC20Permit.sol#L59)).

#### Findings:
```

2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::50 => bytes32 private DOMAIN_SEPARATOR;
```


### [N05] File is missing NatSpec


#### Findings:
```
2022-10-thegraph/contracts/curation/ICuration.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/curation/IGraphCurationToken.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/epochs/IEpochManager.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/gateway/ICallhookReceiver.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/governance/Governed.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/governance/IController.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/governance/Managed.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/governance/Pausable.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/rewards/IRewardsManager.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/staking/IStakingData.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/staking/Staking.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/token/IGraphToken.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/upgrades/IGraphProxy.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
```



### [N06] NC-assembly method available

#### Impact
assembly{ id := chainid() } => uint256 id = block.chainid, assembly { size := extcodesize() } => uint256 size = address().code.length
#### Findings:
```
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::199 => id := chainid()
```


### [N07] Event is missing `indexed` fields

#### Impact
Each event should use three indexed fields if there are three or more fields
#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::56 => event ArbitrumAddressesSet(address inbox, address l1Router);
2022-10-thegraph/contracts/governance/Governed.sol::17 => event NewPendingOwnership(address indexed from, address indexed to);
2022-10-thegraph/contracts/governance/Governed.sol::18 => event NewOwnership(address indexed from, address indexed to);
2022-10-thegraph/contracts/governance/Managed.sol::39 => event ContractSynced(bytes32 indexed nameHash, address contractAddress);
2022-10-thegraph/contracts/governance/Pausable.sol::21 => event NewPauseGuardian(address indexed oldPauseGuardian, address indexed pauseGuardian);
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::24 => event BridgeMinted(address indexed account, uint256 amount);
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::26 => event BridgeBurned(address indexed account, uint256 amount);
2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol::56 => event AdminUpdated(address indexed oldAdmin, address indexed newAdmin);
```


### [N08] Use of sensitive/NC-inclusive terms


#### Findings:
```
2022-10-thegraph/contracts/governance/Pausable.sol::8 => bool internal _partialPaused;
2022-10-thegraph/contracts/governance/Pausable.sol::10 => bool internal _paused;
```







### [N09] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from public to external.
#### Findings:
```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::332 => ) public pure returns (bytes memory) {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::144 => ) public payable override notPaused nonReentrant returns (bytes memory) {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::203 => function calculateL2TokenAddress(address l1ERC20) public view override returns (address) {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::267 => ) public pure returns (bytes memory) {
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::141 => function isMinter(address _account) public view returns (bool) {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::30 => function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::43 => function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::55 => function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::68 => function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::77 => function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::86 => function acceptProxy(GraphUpgradeable _implementation, IGraphProxy _proxy) public onlyGovernor {
```


### [N10] Numeric values having to do with time should use time units for readability

#### Impact
There are units for seconds, minutes, hours, days, and weeks
#### Findings:
```
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::125 => * The tokens will be available on L1 only after the wait period (7 days) is over,
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::179 => * The tokens will be received on L1 only after the wait period (7 days) is over,
```


### [N11] Constant redefined elsewhere

#### Impact
Consider defining in only one contract so that values cannot become out of sync when only one location is updated
#### Findings:
```
2022-10-thegraph/contracts/governance/Managed.sol::174 => bytes32 nameHash = keccak256(abi.encodePacked(_name));
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::38 => bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::39 => bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::83 => bytes32 digest = keccak256(
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::53 => bytes32(uint256(keccak256("eip1967.proxy.pendingImplementation")) - 1)
```


### [N12] Non-library/interface files should use fixed compiler versions, not floating ones

#### Impact

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