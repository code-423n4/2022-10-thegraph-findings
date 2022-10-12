# Low

# Details
## 1. Outdated pragma
### Description
The project is using the solidity version 0.7.6. 
It's a best practice to use the latest release version. 
You can consult it in the following [link](https://github.com/ethereum/solidity/releases)

### Mitigation 
Update the solidity version to 0.8.17

### Lines in the code
[BridgeEscrow.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L3)
[GraphUpgradeable.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L3)
[Governed.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L3)
[Pausable.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L3)
[L2GraphToken.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L3)
[GraphProxyAdmin.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L3)
[GraphProxyStorage.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L3)
[GraphProxy.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L3)
[Managed.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L3)
[GraphTokenUpgradeable.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L3)
[L2GraphTokenGateway.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L3)
[L1GraphTokenGateway.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L3)
[GraphTokenGateway.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L3)
[IGraphCurationToken.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/IGraphCurationToken.sol#L3)
[ICallhookReceiver.sol#L9](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/ICallhookReceiver.sol#L9)
[IGraphProxy.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L3)
[IEpochManager.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L3)
[IController.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L3)
[IGraphToken.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L3)
[IRewardsManager.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L3)
[IStakingData.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStakingData.sol#L3)
[ICuration.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L3)
[IStaking.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L3)


## 2. Floating pragma 
### Description
Use a pragma ^ or >= <= is not a good practice.  
The majority contracts have the pragma solidity directive ^0.7.6. It is recommended to specify a fixed compiler version to ensure that the bytecode produced 
does not vary between builds. 
This is especially important if you rely on bytecode-level verification of the code.


### Mitigation 
Lock the pragma version

### Lines in the code
[BridgeEscrow.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L3)
[GraphUpgradeable.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L3)
[Governed.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L3)
[Pausable.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L3)
[L2GraphToken.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L3)
[GraphProxyAdmin.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L3)
[GraphProxyStorage.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L3)
[GraphProxy.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L3)
[Managed.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L3)
[GraphTokenUpgradeable.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L3)
[L2GraphTokenGateway.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L3)
[L1GraphTokenGateway.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L3)
[GraphTokenGateway.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L3)
[IGraphCurationToken.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/IGraphCurationToken.sol#L3)
[ICallhookReceiver.sol#L9](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/ICallhookReceiver.sol#L9)
[IGraphProxy.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol#L3)
[IEpochManager.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol#L3)
[IController.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol#L3)
[IGraphToken.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol#L3)
[IRewardsManager.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol#L3)
[IStakingData.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStakingData.sol#L3)
[ICuration.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol#L3)
[IStaking.sol#L3](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol#L3)

## 3. Missing checks for address(0x0) when assigning values to address state variables

### Mitigation
Add check for address(0x0)

### Lines in the code
[Governed.sol#L32](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L32)


# Non Critical

## 4. public functions not called by the contract should be declared external instead

[GraphProxyAdmin.sol#L30](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L30)
[GraphProxyAdmin.sol#L43](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L43)
[GraphProxyAdmin.sol#L55](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L55)
[GraphProxyAdmin.sol#L68](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L68)
[GraphProxyAdmin.sol#L77](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L77)
[GraphProxyAdmin.sol#L86](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L86)


## 5. File is missing NatSpec
[BridgeEscrow.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol)
[GraphUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol)
[Governed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol)
[Pausable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol)
[L2GraphToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol)
[GraphProxyAdmin.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol)
[GraphProxyStorage.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol)
[GraphProxy.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol)
[Managed.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol)
[GraphTokenUpgradeable.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol)
[L2GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol)
[L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol)
[GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol)
[IGraphCurationToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/IGraphCurationToken.sol)
[ICallhookReceiver.sol#L9](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/ICallhookReceiver.sol#L9)
[IGraphProxy.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/IGraphProxy.sol)
[IEpochManager.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/IEpochManager.sol)
[IController.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/IController.sol)
[IGraphToken.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/IGraphToken.sol)
[IRewardsManager.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/IRewardsManager.sol)
[IStakingData.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStakingData.sol)
[ICuration.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/ICuration.sol)
[IStaking.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/IStaking.sol)

## 6. Event is missing indexed fields
### Description
Index event fields make the field more quickly accessible to off-chain tools that parse events. 
However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (threefields). 
Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. 
If there are fewer than three fields, all of the fields should be indexed.

### Lines in the code
[L2GraphToken.sol#L24-L30](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L24-L30)
[Pausable.sol#L19-L20](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L19-L20)
[L1GraphTokenGateway.sol#L56-L66](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L56-L66)
[Managed.sol#L33-L34](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L33-L34)
[Managed.sol#L39](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L39)
[L2GraphTokenGateway.sol#L58-L62](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L58-L62)

