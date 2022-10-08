# Qa Report

## THEGRAPH

### Use `external` visibility modifier for function that are not being invoked by the contract

[GraphProxyAdmin.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol):

```solidity
line#L30:  function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {

line#L43:  function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {

line#L68:  function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {

line#L77:  function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {
```

[Managed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol):

```solidity
line#L18:  * public `syncAllContracts()` function whenever a contract changes in the controller.
```

### Use higher solidity version

Optimizations, bug fixes and additional features come with each solidity `patch` version update

[Managed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[GraphUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[IEpochManager.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/epochs/IEpochManager.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[IController.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/IController.sol):

```solidity
line#L3:   pragma solidity >=0.6.12 <0.8.0;
```

[IGraphCurationToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/IGraphCurationToken.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[L2GraphToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[GraphProxyStorage.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[L1ArbitrumMessenger.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol):

```solidity
line#L26:  pragma solidity ^0.7.6;
```

[L2ArbitrumMessenger.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L2ArbitrumMessenger.sol):

```solidity
line#L26:  pragma solidity ^0.7.6;
```

[IGraphProxy.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/IGraphProxy.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[GraphProxy.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[AddressAliasHelper.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/AddressAliasHelper.sol):

```solidity
line#L26:  pragma solidity ^0.7.6;
```

[IStakingData.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStakingData.sol):

```solidity
line#L3:   pragma solidity >=0.6.12 <0.8.0;
```

[ICallhookReceiver.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/ICallhookReceiver.sol):

```solidity
line#L9:   pragma solidity ^0.7.6;
```

[IStaking.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol):

```solidity
line#L3:   pragma solidity >=0.6.12 <0.8.0;
```

[Pausable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[L2GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[BridgeEscrow.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[IRewardsManager.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/rewards/IRewardsManager.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[IGraphToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/token/IGraphToken.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[Governed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[GraphProxyAdmin.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[ICuration.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

### Unused import statements

[ICuration.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol):

```solidity
line#L5:   import "./IGraphCurationToken.sol";
```

### Missing natspec comments

https://docs.soliditylang.org/en/develop/natspec-format.html

[IGraphCurationToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/IGraphCurationToken.sol):

```solidity
line#L1:   // SPDX-License-Identifier: GPL-2.0-or-later
```

[IGraphProxy.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/IGraphProxy.sol):

```solidity
line#L1:   // SPDX-License-Identifier: GPL-2.0-or-later
```

[IGraphToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/token/IGraphToken.sol):

```solidity
line#L1:   // SPDX-License-Identifier: GPL-2.0-or-later
```

[IEpochManager.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/epochs/IEpochManager.sol):

```solidity
line#L1:   // SPDX-License-Identifier: GPL-2.0-or-later
```

[IController.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/IController.sol):

```solidity
line#L1:   // SPDX-License-Identifier: GPL-2.0-or-later
```

[ICuration.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol):

```solidity
line#L1:   // SPDX-License-Identifier: GPL-2.0-or-later
```

### Events should be emmitted on every critical state changes

Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

[GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol):

```solidity
line#L150: function _initialize(address _owner, uint256 _initialSupply) internal {
```

[Governed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol):

```solidity
line#L31:  function _initialize(address _initGovernor) internal {
```

### Contracts should use a fixed compiler version to avoid potential bugs

[L1ArbitrumMessenger.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol):

```solidity
line#L26:  pragma solidity ^0.7.6;
```

[GraphProxyStorage.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[Managed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[GraphProxyAdmin.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[BridgeEscrow.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[ICallhookReceiver.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/ICallhookReceiver.sol):

```solidity
line#L9:   pragma solidity ^0.7.6;
```

[L2GraphToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[Pausable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[GraphUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[Governed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[L2ArbitrumMessenger.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L2ArbitrumMessenger.sol):

```solidity
line#L26:  pragma solidity ^0.7.6;
```

[GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[L2GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```

[GraphProxy.sol](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol):

```solidity
line#L3:   pragma solidity ^0.7.6;
```
