## [L-01] Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

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

## [L-02] Use of Block.timestamp

Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.

```
2022-10-thegraph/contracts/governance/Pausable.sol::32 => lastPausePartialTime = block.timestamp;
2022-10-thegraph/contracts/governance/Pausable.sol::46 => lastPauseTime = block.timestamp;
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::95 => require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
2022-10-thegraph/contracts/token/GraphToken.sol::118 => require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```

## [L-03] abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). Unless there is a compelling reason, abi.encode should be preferred. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

```
2022-10-thegraph/contracts/governance/Managed.sol::174 => bytes32 nameHash = keccak256(abi.encodePacked(_name));
2022-10-thegraph/contracts/staking/Staking.sol::1115 => bytes32 messageHash = keccak256(abi.encodePacked(_indexer, _allocationID));
```

## [L-04] require() should be used instead of assert()

require() should be used for checking error conditions on inputs and return values while assert() should be used for invariant checking

```
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::47 => assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::48 => assert(
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::51 => assert(
```

## [L-05] approve should be replaced with safeApprove or safeIncreaseAllowance() / safeDecreaseAllowance()

approve is subject to a known front-running attack. Consider using safeApprove() or safeIncreaseAllowance() or safeDecreaseAllowance() instead

```
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::29 => graphToken().approve(_spender, type(uint256).max);
```

## [L-06] Unsafe use of transfer()/transferFrom() with IERC20

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::235 => token.transferFrom(from, escrow, _amount);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::276 => token.transferFrom(escrow, _to, _amount);
```

## [L-07] _safeMint() should be used rather than _mint() wherever possible

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin’s SafeERC20's safeTransfer()/safeTransferFrom() instead

```
2022-10-thegraph/contracts/curation/GraphCurationToken.sol::37 => _mint(_to, _amount);
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::133 => _mint(_to, _amount);
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::155 => _mint(_owner, _initialSupply);
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::81 => _mint(_account, _amount);
2022-10-thegraph/contracts/token/GraphToken.sol::68 => _mint(msg.sender, _initialSupply);
2022-10-thegraph/contracts/token/GraphToken.sol::152 => _mint(_to, _amount);
```

## [L-08] Upgradeable contract is missing a __gap[50] storage variable to allow for new storage variables in later versions

__gap is empty reserved space in storage that is recommended to be put in place in upgradeable contracts. It allows new state variables to be added in the future without compromising the storage compatibility with existing deployments

```
2022-10-thegraph/contracts/curation/Curation.sol::29 => contract Curation is CurationV1Storage, GraphUpgradeable {
2022-10-thegraph/contracts/curation/GraphCurationToken.sol::5 => import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";
2022-10-thegraph/contracts/curation/IGraphCurationToken.sol::5 => import "@openzeppelin/contracts-upgradeable/token/ERC20/IERC20Upgradeable.sol";
2022-10-thegraph/contracts/epochs/EpochManager.sol::16 => contract EpochManager is EpochManagerV1Storage, GraphUpgradeable, IEpochManager {
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::15 => contract BridgeEscrow is GraphUpgradeable, Managed {
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::14 => abstract contract GraphTokenGateway is GraphUpgradeable, Pausable, Managed, ITokenGateway {
2022-10-thegraph/contracts/governance/GraphGovernance.sol::13 => contract GraphGovernance is GraphGovernanceV1Storage, GraphUpgradeable, IGraphGovernance {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::6 => import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::5 => import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20BurnableUpgradeable.sol";
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::15 => contract L2GraphToken is GraphTokenUpgradeable, IArbToken {
2022-10-thegraph/contracts/rewards/RewardsManager.sol::31 => contract RewardsManager is RewardsManagerV3Storage, GraphUpgradeable, IRewardsManager {
2022-10-thegraph/contracts/staking/Staking.sol::24 => contract Staking is StakingV2Storage, GraphUpgradeable, IStaking, Multicall {
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::13 => * This contract implements a proxy that is upgradeable by an admin.
2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol::9 => * @dev This contract is intended to be inherited from upgradeable contracts.
```

## [L-09] Implementation contract may not be initialized

Implementation contract does not have a constructor with the initializer modifier therefore may be uninitialized. Implementation contracts should be initialized to avoid potential griefs or exploits.

```
2022-10-thegraph/contracts/curation/Curation.sol::29 => contract Curation is CurationV1Storage, GraphUpgradeable {
2022-10-thegraph/contracts/curation/GraphCurationToken.sol::5 => import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";
2022-10-thegraph/contracts/curation/IGraphCurationToken.sol::5 => import "@openzeppelin/contracts-upgradeable/token/ERC20/IERC20Upgradeable.sol";
2022-10-thegraph/contracts/epochs/EpochManager.sol::16 => contract EpochManager is EpochManagerV1Storage, GraphUpgradeable, IEpochManager {
2022-10-thegraph/contracts/gateway/BridgeEscrow.sol::15 => contract BridgeEscrow is GraphUpgradeable, Managed {
2022-10-thegraph/contracts/gateway/GraphTokenGateway.sol::14 => abstract contract GraphTokenGateway is GraphUpgradeable, Pausable, Managed, ITokenGateway {
2022-10-thegraph/contracts/governance/GraphGovernance.sol::13 => contract GraphGovernance is GraphGovernanceV1Storage, GraphUpgradeable, IGraphGovernance {
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::6 => import "@openzeppelin/contracts-upgradeable/utils/ReentrancyGuardUpgradeable.sol";
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::15 => contract L2GraphToken is GraphTokenUpgradeable, IArbToken {
2022-10-thegraph/contracts/rewards/RewardsManager.sol::31 => contract RewardsManager is RewardsManagerV3Storage, GraphUpgradeable, IRewardsManager {
2022-10-thegraph/contracts/staking/Staking.sol::24 => contract Staking is StakingV2Storage, GraphUpgradeable, IStaking, Multicall {
```

## [N-1] Use a more recent version of solidity

Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

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

## [N-2] require()/revert() statements should have descriptive reason strings

Descriptive reason strings should be used so that users can troubleshot any reverted calls

```
2022-10-thegraph/contracts/upgrades/GraphProxy.sol::133 => require(success);
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::34 => require(success);
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::47 => require(success);
2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol::59 => require(success);
```

## [N-3] Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

```
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::56 => event ArbitrumAddressesSet(address inbox, address l1Router);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::58 => event L2TokenAddressSet(address l2GRT);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::60 => event L2CounterpartAddressSet(address l2Counterpart);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::62 => event EscrowAddressSet(address escrow);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::64 => event AddedToCallhookWhitelist(address newWhitelisted);
2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol::66 => event RemovedFromCallhookWhitelist(address notWhitelisted);
2022-10-thegraph/contracts/governance/Managed.sol::33 => event ParameterUpdated(string param);
2022-10-thegraph/contracts/governance/Managed.sol::34 => event SetController(address controller);
2022-10-thegraph/contracts/governance/Pausable.sol::19 => event PartialPauseChanged(bool isPaused);
2022-10-thegraph/contracts/governance/Pausable.sol::20 => event PauseChanged(bool isPaused);
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::58 => event L2RouterSet(address l2Router);
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::60 => event L1TokenAddressSet(address l1GRT);
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::62 => event L1CounterpartAddressSet(address l1Counterpart);
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::28 => event GatewaySet(address gateway);
2022-10-thegraph/contracts/l2/token/L2GraphToken.sol::30 => event L1AddressSet(address l1Address);
```

## [N-4] Missing SPDX license identifier

File should include SPDX-License-Identifier

```
2022-10-thegraph/contracts/staking/libs/Cobbs.sol::1 => /*
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::1 => /*
```

## [N-5] Missing NatSpec

Code should include NatSpec

```
2022-10-thegraph/contracts/curation/CurationStorage.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/curation/ICuration.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/curation/IGraphCurationToken.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/epochs/EpochManagerStorage.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/epochs/IEpochManager.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/governance/GraphGovernanceStorage.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/governance/IController.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/governance/IManaged.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/rewards/RewardsManagerStorage.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/staking/StakingStorage.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/token/IGraphToken.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
2022-10-thegraph/contracts/upgrades/IGraphProxy.sol::1 => // SPDX-License-Identifier: GPL-2.0-or-later
```

## [N-6] Public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents' functions and change the visibility from external to public.

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

## [N-7] Adding a return statement when the function defines a named return variable, is redundant

It is not necessary to have both a named return and a return statement.

```
2022-10-thegraph/contracts/l2/gateway/L2GraphTokenGateway.sol::286 => function parseOutboundData(bytes memory _data) private view returns (address, bytes memory) {
2022-10-thegraph/contracts/staking/libs/Cobbs.sol::51 => ) public pure returns (uint256 rewards) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::146 => function toInteger(int256 f) internal pure returns (int256 n) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::151 => function ln(int256 x) internal pure returns (int256 r) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::258 => function exp(int256 x) internal pure returns (int256 r) {
2022-10-thegraph/contracts/staking/libs/LibFixedMath.sol::380 => function _mul(int256 a, int256 b) private pure returns (int256 c) {
```

## [N-08] Non-assembly method available

assembly{ id := chainid() } => uint256 id = block.chainid
assembly { size := extcodesize() } => uint256 size = address().code.length

```
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::199 => id := chainid()
2022-10-thegraph/contracts/token/GraphToken.sol::189 => id := chainid()
```

## [N-09] Expressions for constant values such as a call to keccak256(), should use immutable rather than constant

instances:

```
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::38 => bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
2022-10-thegraph/contracts/l2/token/GraphTokenUpgradeable.sol::39 => bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
2022-10-thegraph/contracts/token/GraphToken.sol::35 => bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
2022-10-thegraph/contracts/token/GraphToken.sol::36 => bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
```
