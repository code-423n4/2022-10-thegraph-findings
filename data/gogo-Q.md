# 2022-10-THEGRAPH

## Low Risk and Non-Critical Issues

### Events not emmited on important state changes

Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.

_There are **2** instances of this issue:_

```solidity
File: contracts/governance/Governed.sol

31:   function _initialize(address _initGovernor) internal {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

150:  function _initialize(address _owner, uint256 _initialSupply) internal {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

### `public` functions not called by the contract should be declared `external` instead

_There are **6** instances of this issue:_

```solidity
File: contracts/governance/Managed.sol

18:   * public `syncAllContracts()` function whenever a contract changes in the controller.
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol

```solidity
File: contracts/upgrades/GraphProxyAdmin.sol

30:   function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {

43:   function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {

55:   function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {

68:   function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {

77:   function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol

### Non-library/interface files should use fixed compiler versions, not floating ones

_There are **16** instances of this issue:_

```solidity
File: contracts/arbitrum/L1ArbitrumMessenger.sol

26:   pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol

```solidity
File: contracts/arbitrum/L2ArbitrumMessenger.sol

26:   pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L2ArbitrumMessenger.sol

```solidity
File: contracts/gateway/BridgeEscrow.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol

```solidity
File: contracts/gateway/GraphTokenGateway.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol

```solidity
File: contracts/gateway/ICallhookReceiver.sol

9:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/ICallhookReceiver.sol

```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

```solidity
File: contracts/governance/Governed.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol

```solidity
File: contracts/governance/Managed.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol

```solidity
File: contracts/governance/Pausable.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

```solidity
File: contracts/l2/token/L2GraphToken.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol

```solidity
File: contracts/upgrades/GraphProxy.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol

```solidity
File: contracts/upgrades/GraphProxyAdmin.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol

```solidity
File: contracts/upgrades/GraphProxyStorage.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol

```solidity
File: contracts/upgrades/GraphUpgradeable.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol

### Natspec is missing

_There are **6** instances of this issue:_

```solidity
File: contracts/curation/ICuration.sol

1:    // SPDX-License-Identifier: GPL-2.0-or-later
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol

```solidity
File: contracts/curation/IGraphCurationToken.sol

1:    // SPDX-License-Identifier: GPL-2.0-or-later
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/IGraphCurationToken.sol

```solidity
File: contracts/epochs/IEpochManager.sol

1:    // SPDX-License-Identifier: GPL-2.0-or-later
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/epochs/IEpochManager.sol

```solidity
File: contracts/governance/IController.sol

1:    // SPDX-License-Identifier: GPL-2.0-or-later
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/IController.sol

```solidity
File: contracts/token/IGraphToken.sol

1:    // SPDX-License-Identifier: GPL-2.0-or-later
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/token/IGraphToken.sol

```solidity
File: contracts/upgrades/IGraphProxy.sol

1:    // SPDX-License-Identifier: GPL-2.0-or-later
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/IGraphProxy.sol

### Not used import

_There are **1** instances of this issue:_

```solidity
File: contracts/curation/ICuration.sol

5:    import "./IGraphCurationToken.sol";
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol

### Use a more recent version of solidity

- Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining \ - Use a solidity version of at least 0.8.3 to get cheaper multiple storage reads \ - Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings \ - Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

_There are **26** instances of this issue:_

```solidity
File: contracts/arbitrum/AddressAliasHelper.sol

26:   pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/AddressAliasHelper.sol

```solidity
File: contracts/arbitrum/L1ArbitrumMessenger.sol

26:   pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol

```solidity
File: contracts/arbitrum/L2ArbitrumMessenger.sol

26:   pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L2ArbitrumMessenger.sol

```solidity
File: contracts/curation/ICuration.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol

```solidity
File: contracts/curation/IGraphCurationToken.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/IGraphCurationToken.sol

```solidity
File: contracts/epochs/IEpochManager.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/epochs/IEpochManager.sol

```solidity
File: contracts/gateway/BridgeEscrow.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol

```solidity
File: contracts/gateway/GraphTokenGateway.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol

```solidity
File: contracts/gateway/ICallhookReceiver.sol

9:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/ICallhookReceiver.sol

```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

```solidity
File: contracts/governance/Governed.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol

```solidity
File: contracts/governance/IController.sol

3:    pragma solidity >=0.6.12 <0.8.0;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/IController.sol

```solidity
File: contracts/governance/Managed.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol

```solidity
File: contracts/governance/Pausable.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

```solidity
File: contracts/l2/token/L2GraphToken.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol

```solidity
File: contracts/rewards/IRewardsManager.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/rewards/IRewardsManager.sol

```solidity
File: contracts/staking/IStaking.sol

3:    pragma solidity >=0.6.12 <0.8.0;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol

```solidity
File: contracts/staking/IStakingData.sol

3:    pragma solidity >=0.6.12 <0.8.0;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStakingData.sol

```solidity
File: contracts/token/IGraphToken.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/token/IGraphToken.sol

```solidity
File: contracts/upgrades/GraphProxy.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol

```solidity
File: contracts/upgrades/GraphProxyAdmin.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol

```solidity
File: contracts/upgrades/GraphProxyStorage.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol

```solidity
File: contracts/upgrades/GraphUpgradeable.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol

```solidity
File: contracts/upgrades/IGraphProxy.sol

3:    pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/IGraphProxy.sol
