# USE A MORE RECENT VERSION OF SOLIDITY

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/IGraphCurationToken.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/ICallhookReceiver.sol#L9

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol#L26

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/epochs/IEpochManager.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/IGraphProxy.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/IController.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/token/IGraphToken.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/AddressAliasHelper.sol#L26

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStakingData.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L2ArbitrumMessenger.sol#L26

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/rewards/IRewardsManager.sol#L3

# EVENTS NOT EMMITED ON IMPORTANT STATE CHANGES

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L31

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L150

# NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol#L26

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/ICallhookReceiver.sol#L9

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L3

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L2ArbitrumMessenger.sol#L26

# PUBLIC FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE DECLARED EXTERNAL INSTEAD

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L18

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L30 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L55 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L68 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L77
