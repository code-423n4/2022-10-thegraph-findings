# GAS REPORT

## [GAS] Consider using custom errors
You can utilize customized errors in require statements to save gas.

### Proof of concept:
- [Staking.sol#L1120](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/Staking.sol#L1120)
- [EpochManager.sol#L46](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/EpochManager.sol#L46)

## [GAS] The following require statements could be split into multiple require statements to save gas


### Proof of concept:
- [L1GraphTokenGateway.sol#L141](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L141)
- [Staking.sol#L368](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/Staking.sol#L368)

## [GAS] Caching msg.sender is unnecessary


### Proof of concept:
- [BancorFormula.sol#L236](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/bancor/BancorFormula.sol#L236)
- [SubgraphNFT.sol#L46](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/discovery/SubgraphNFT.sol#L46)

## [GAS] Cache the following arrays size before the loop


### Proof of concept:
- [Staking.sol#L1049](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/Staking.sol#L1049)
- [AllocationExchange.sol#L135](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/statechannels/AllocationExchange.sol#L135)

--