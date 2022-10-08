## USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.0 to get overflow protection without `LowGasSafeMath` Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than `revert()/require()` strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

Instances include:
[`contracts/gateway/BridgeEscrow.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L3)
[`contracts/upgrades/GraphUpgradeable.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L3)
[`contracts/governance/Governed.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L3)
[`contracts/governance/Pausable.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L3)
[`contracts/l2/token/L2GraphToken.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L3)
[`contracts/upgrades/GraphProxyAdmin.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L3)
[`contracts/upgrades/GraphProxyStorage.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L3)
[`contracts/upgrades/GraphProxy.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L3)
[`contracts/governance/Managed.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L3)
[`contracts/l2/token/GraphTokenUpgradeable.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L3)
[`contracts/l2/gateway/L2GraphTokenGateway.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L3)
[`contracts/gateway/L1GraphTokenGateway.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L3)
[`contracts/gateway/GraphTokenGateway.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L3)
[`contracts/curation/IGraphCurationToken.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/IGraphCurationToken.sol#L3)
[`contracts/gateway/ICallhookReceiver.sol:9`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/ICallhookReceiver.sol#L9)
[`contracts/upgrades/IGraphProxy.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/IGraphProxy.sol#L3)
[`contracts/epochs/IEpochManager.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/epochs/IEpochManager.sol#L3)
[`contracts/governance/IController.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/IController.sol#L3)
[`contracts/token/IGraphToken.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/token/IGraphToken.sol#L3)
[`contracts/rewards/IRewardsManager.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/rewards/IRewardsManager.sol#L3)
[`contracts/staking/IStakingData.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStakingData.sol#L3)
[`contracts/curation/ICuration.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol#L3)
[`contracts/staking/IStaking.sol:3`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L3)

## Require/Revert strings longer than 32 bytes cost additional gas
Instances include:
[`contracts/upgrades/GraphUpgradeable.sol:32`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L32)
[`contracts/upgrades/GraphProxy.sol:105`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L105)
[`contracts/upgrades/GraphProxy.sol:141`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L141)
[`contracts/upgrades/GraphProxy.sol:144`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L144)
[`contracts/governance/Managed.sol:53`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L53)
[`contracts/gateway/GraphTokenGateway.sol:21`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L21)

## Use assembly to check for address(0)
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

Instances include:
[`contracts/governance/Governed.sol:41`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L41)
[`contracts/governance/Governed.sol:55`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L55)
[`contracts/l2/token/L2GraphToken.sol:49`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L49)
[`contracts/l2/token/L2GraphToken.sol:60`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L60)
[`contracts/l2/token/L2GraphToken.sol:70`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L70)
[`contracts/upgrades/GraphProxy.sol:105`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L105)
[`contracts/upgrades/GraphProxy.sol:143`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L143)
[`contracts/governance/Managed.sol:104`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L104)
[`contracts/l2/token/GraphTokenUpgradeable.sol:106`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L106)
[`contracts/l2/gateway/L2GraphTokenGateway.sol:98`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L98)
[`contracts/l2/gateway/L2GraphTokenGateway.sol:108`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L108)
[`contracts/l2/gateway/L2GraphTokenGateway.sol:118`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L118)
[`contracts/l2/gateway/L2GraphTokenGateway.sol:148`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L148)
[`contracts/gateway/L1GraphTokenGateway.sol:74`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L74)
[`contracts/gateway/L1GraphTokenGateway.sol:110`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L110)
[`contracts/gateway/L1GraphTokenGateway.sol:111`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L111)
[`contracts/gateway/L1GraphTokenGateway.sol:122`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L122)
[`contracts/gateway/L1GraphTokenGateway.sol:132`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L132)
[`contracts/gateway/L1GraphTokenGateway.sol:142`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142)
[`contracts/gateway/L1GraphTokenGateway.sol:153`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L153)
[`contracts/gateway/L1GraphTokenGateway.sol:165`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L165)
[`contracts/gateway/L1GraphTokenGateway.sol:202`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L202)
[`contracts/gateway/GraphTokenGateway.sol:31`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L31)

## In `require()`, Use `!= 0` Instead of `> 0` With Uint Values  
  
In a require, when checking a uint, using `!= 0` instead of `> 0` saves 6 gas. This will jump over or avoid an extra `ISZERO` opcode.  
  
Instances include:  
[`contracts/gateway/L1GraphTokenGateway.sol:201`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L201)
[`contracts/gateway/L1GraphTokenGateway.sol:217`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L217)

## Splitting require() statements that use && saves gas

Saves 16 gas per instance. If you're using the Optimizer at 200, instead of using the && operator in a single require statement to check multiple conditions, multiple require statements with 1 condition per require statement should be used to save gas:

[`contracts/governance/Governed.sol:55`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L55)
[`contracts/upgrades/GraphProxy.sol:143`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L143)
[`contracts/l2/gateway/L2GraphTokenGateway.sol:146`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L146)
[`contracts/gateway/L1GraphTokenGateway.sol:142`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142)

## Calls to `keccak256` should use `immutable` instead of constants

Instances include:
[`contracts/l2/token/GraphTokenUpgradeable.sol:34`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L34)
[`contracts/l2/token/GraphTokenUpgradeable.sol:38`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L38)
[`contracts/l2/token/GraphTokenUpgradeable.sol:39`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L39)
[`contracts/l2/token/GraphTokenUpgradeable.sol:40`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L40)
[`contracts/l2/token/GraphTokenUpgradeable.sol:42`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L42)

## `abi.encode()` is less efficient than `abi.encodepacked()`
Instances include:
[`contracts/l2/token/GraphTokenUpgradeable.sol:162`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L162)
[`contracts/l2/token/GraphTokenUpgradeable.sol:88`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L88)
[`contracts/l2/gateway/L2GraphTokenGateway.sol:174`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L174)
[`contracts/l2/gateway/L2GraphTokenGateway.sol:275`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L275)
[`contracts/gateway/L1GraphTokenGateway.sol:249`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L249)
[`contracts/gateway/L1GraphTokenGateway.sol:342`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L342)

