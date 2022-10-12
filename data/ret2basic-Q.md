# The Graph L2 Bridge Contest QA Report

## Summary

The following QA issues were found during the code review:

1. Unsafe ERC20 Operation(s) (3 instances)
2. Unspecific Compiler Version Pragma (23 instances)

Total 26 instances of 2 issues.

## 1. Unsafe ERC20 Operation(s) (3 instances)

ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard. It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.

```solidity
contracts/gateway/BridgeEscrow.sol::29 => graphToken().approve(_spender, type(uint256).max);

contracts/gateway/L1GraphTokenGateway.sol::235 => token.transferFrom(from, escrow, _amount);

contracts/gateway/L1GraphTokenGateway.sol::276 => token.transferFrom(escrow, _to, _amount);
```

## 2. Unspecific Compiler Version Pragma (23 instances)

Avoid floating pragmas for non-library contracts. While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations. A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain. It is recommended to pin to a concrete compiler version.

```solidity
contracts/gateway/BridgeEscrow.sol::3 => pragma solidity ^0.7.6;

contracts/upgrades/GraphUpgradeable.sol::3 => pragma solidity ^0.7.6;

contracts/governance/Governed.sol::3 => pragma solidity ^0.7.6;

contracts/governance/Pausable.sol::3 => pragma solidity ^0.7.6;

contracts/l2/token/L2GraphToken.sol::3 => pragma solidity ^0.7.6;

contracts/upgrades/GraphProxyAdmin.sol::3 => pragma solidity ^0.7.6;

contracts/upgrades/GraphProxyStorage.sol::3 => pragma solidity ^0.7.6;

contracts/upgrades/GraphProxy.sol::3 => pragma solidity ^0.7.6;

contracts/governance/Managed.sol::3 => pragma solidity ^0.7.6;

contracts/l2/token/GraphTokenUpgradeable.sol::3 => pragma solidity ^0.7.6;

contracts/l2/gateway/L2GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;

contracts/gateway/L1GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;

contracts/gateway/GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;

contracts/curation/IGraphCurationToken.sol::3 => pragma solidity ^0.7.6;

contracts/gateway/ICallhookReceiver.sol::9 => pragma solidity ^0.7.6;

contracts/upgrades/IGraphProxy.sol::3 => pragma solidity ^0.7.6;

contracts/epochs/IEpochManager.sol::3 => pragma solidity ^0.7.6;

contracts/governance/IController.sol::3 => pragma solidity >=0.6.12 <0.8.0;

contracts/token/IGraphToken.sol::3 => pragma solidity ^0.7.6;

contracts/rewards/IRewardsManager.sol::3 => pragma solidity ^0.7.6;

contracts/staking/IStakingData.sol::3 => pragma solidity >=0.6.12 <0.8.0;

contracts/curation/ICuration.sol::3 => pragma solidity ^0.7.6;

contracts/staking/IStaking.sol::3 => pragma solidity >=0.6.12 <0.8.0;
```
