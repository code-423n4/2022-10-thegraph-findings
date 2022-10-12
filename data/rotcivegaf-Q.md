# QA report

## Low Risk
| L-N    |Issue|
|:------:|:----|
| [L&#x2011;01] | Wrong event parameter emit |
| [L&#x2011;02] | With only admin role can't `acceptUpgrade` or `acceptUpgradeAndCall` |
| [L&#x2011;03] | Mark as abstract |

## Non-critical

| N-N    |Issue|
|:------:|:----|
| [N&#x2011;01] | Missing error message in `require` statement |
| [N&#x2011;02] | Unused imports |
| [N&#x2011;03] | Non-library/interface files should use fixed compiler versions, not floating ones |
| [N&#x2011;04] | Wrong function doc |

## Low Risk

### [L-01] Wrong event parameter emit

The `_admin()` will be the new admin and not the old, save the old admin in a var and emit this one, like in [`_setImplementation`](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L115-L124) or [`_setPendingImplementation` ](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L130-L139) functions

```solidity
File: /contracts/upgrades/GraphProxyStorage.sol

86        emit AdminUpdated(_admin(), _newAdmin);
```

### [L-02] With only admin role can't `acceptUpgrade` or `acceptUpgradeAndCall`

The functions `acceptUpgrade` and `acceptUpgradeAndCall` have the `ifAdminOrPendingImpl` modifier, but it the sender it's the `_admin` the transcaction recerts the `msg.sender == _pendingImplementation` inside the `_acceptUpgrade` function

```solidity
File: /contracts/upgrades/GraphProxy.sol

122    function acceptUpgrade() external ifAdminOrPendingImpl {

129    function acceptUpgradeAndCall(bytes calldata data) external ifAdminOrPendingImpl {

143            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
```

### [L-03] Mark as abstract

If the **Governed.sol** contract is deployed,  will fail to initialize

```solidity
File: /contracts/governance/Governed.sol

9 contract Governed {
```

Same with the **GraphTokenUpgradeable.sol**

```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol

28 contract GraphTokenUpgradeable is GraphUpgradeable, Governed, ERC20BurnableUpgradeable {
```

```solidity
File: /contracts/upgrades/GraphUpgradeable.sol

11 contract GraphUpgradeable {
```

```solidity
File: /contracts/upgrades/GraphProxyStorage.sol

11 contract GraphProxyStorage {
```

```solidity
File: /contracts/governance/Managed.sol

23 contract Managed {
```


## Non-critical

### [N-01] Missing error message in `require` statements

```solidity
File: /contracts/upgrades/GraphProxy.sol

133        require(success);
```

```solidity
File: /contracts/upgrades/GraphProxyAdmin.sol

34        require(success);

47        require(success);

59        require(success);
```

### [N-02] Unused imports

```solidity
File: /contracts/curation/ICuration.sol

5 import "./IGraphCurationToken.sol";
```

### [N‑03] Non-library/interface files should use fixed compiler versions, not floating ones

```solidity
File: /contracts/gateway/BridgeEscrow.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/upgrades/GraphUpgradeable.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/governance/Governed.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/governance/Pausable.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/l2/token/L2GraphToken.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/upgrades/GraphProxyAdmin.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/upgrades/GraphProxy.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/governance/Managed.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/l2/gateway/L2GraphTokenGateway.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/gateway/L1GraphTokenGateway.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/gateway/GraphTokenGateway.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/curation/IGraphCurationToken.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/gateway/ICallhookReceiver.sol

9 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/upgrades/IGraphProxy.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/epochs/IEpochManager.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/governance/IController.sol

3 pragma solidity >=0.6.12 <0.8.0;
```

```solidity
File: /contracts/token/IGraphToken.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/rewards/IRewardsManager.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/staking/IStakingData.sol

3 pragma solidity >=0.6.12 <0.8.0;
```

```solidity
File: /contracts/curation/ICuration.sol

3 pragma solidity ^0.7.6;
```

```solidity
File: /contracts/staking/IStaking.sol

3 pragma solidity >=0.6.12 <0.8.0;
```

### [N‑04] Wrong function doc

Should be: `* NOTE: Only the admin and implementation can call this function.` like in `admin` function

```solidity
File: /contracts/upgrades/GraphProxy.sol

76     * NOTE: Only the admin can call this function.

89     * NOTE: Only the admin can call this function.
```
