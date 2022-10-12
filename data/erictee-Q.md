### [L-01] ```approve()``` and ```safeApprove()``` should be replaced with ```safeIncreaseAllowance()``` / ```safeDecreaseAllowance()```


#### Impact
```approve()``` & ```safeApprove()``` are deprecated and subject to a known front-running attack. Consider using  ```safeIncreaseAllowance()``` & ```safeDecreaseAllowance()``` instead.


#### Findings:
```
contracts/gateway/BridgeEscrow.sol:L29                 graphToken().approve(_spender, type(uint256).max);

```
### [L-02] ```require()``` should be used instead of ```assert()```


#### Impact
Prior to solidity version 0.8.0, hitting an assert consumes the remainder of the transaction’s available gas rather than returning it, as ```require()```/```revert()``` do. ```assert()``` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that “The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix”.


#### Findings:
```
contracts/upgrades/GraphProxy.sol:L47        assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));

contracts/upgrades/GraphProxy.sol:L48        assert(

contracts/upgrades/GraphProxy.sol:L51        assert(

```
### [L-03] Avoid floating pragmas for non-library contracts.


#### Impact
While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

#### Findings:
```
contracts/token/IGraphToken.sol:L3     pragma solidity ^0.7.6;

contracts/staking/IStakingData.sol:L3     pragma solidity >=0.6.12 <0.8.0;

contracts/staking/IStaking.sol:L3     pragma solidity >=0.6.12 <0.8.0;

contracts/rewards/IRewardsManager.sol:L3     pragma solidity ^0.7.6;

contracts/gateway/L1GraphTokenGateway.sol:L3     pragma solidity ^0.7.6;

contracts/gateway/ICallhookReceiver.sol:L9     pragma solidity ^0.7.6;

contracts/gateway/GraphTokenGateway.sol:L3     pragma solidity ^0.7.6;

contracts/gateway/BridgeEscrow.sol:L3     pragma solidity ^0.7.6;

contracts/upgrades/GraphProxy.sol:L3     pragma solidity ^0.7.6;

contracts/upgrades/GraphUpgradeable.sol:L3     pragma solidity ^0.7.6;

contracts/upgrades/IGraphProxy.sol:L3     pragma solidity ^0.7.6;

contracts/upgrades/GraphProxyAdmin.sol:L3     pragma solidity ^0.7.6;

contracts/upgrades/GraphProxyStorage.sol:L3     pragma solidity ^0.7.6;

contracts/governance/Governed.sol:L3     pragma solidity ^0.7.6;

contracts/governance/Pausable.sol:L3     pragma solidity ^0.7.6;

contracts/governance/IController.sol:L3     pragma solidity >=0.6.12 <0.8.0;

contracts/governance/Managed.sol:L3     pragma solidity ^0.7.6;

contracts/epochs/IEpochManager.sol:L3     pragma solidity ^0.7.6;

contracts/curation/IGraphCurationToken.sol:L3     pragma solidity ^0.7.6;

contracts/curation/ICuration.sol:L3     pragma solidity ^0.7.6;

contracts/l2/token/L2GraphToken.sol:L3     pragma solidity ^0.7.6;

contracts/l2/token/GraphTokenUpgradeable.sol:L3     pragma solidity ^0.7.6;

contracts/l2/gateway/L2GraphTokenGateway.sol:L3     pragma solidity ^0.7.6;

```
### [L-04] ```require()```/```revert()``` statements should have descriptive strings.


#### Impact
Consider adding descriptive strings in ```require()```/```revert()```.


#### Findings:
```
contracts/upgrades/GraphProxy.sol:L133        require(success);

contracts/upgrades/GraphProxyAdmin.sol:L34        require(success);

contracts/upgrades/GraphProxyAdmin.sol:L47        require(success);

contracts/upgrades/GraphProxyAdmin.sol:L59        require(success);

```

### [L-05] ```_safemint()``` should be used rather than ```_mint()``` wherever possible


#### Impact
```_mint()``` is [discouraged](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L271) in favor of ```_safeMint()``` which ensures that the recipient is either an EOA or implements ```IERC721Receiver```. Both [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/d4d8d2ed9798cc3383912a23b5e8d5cb602f7d4b/contracts/token/ERC721/ERC721.sol#L238-L250) and [solmate](https://github.com/transmissions11/solmate/blob/4eaf6b68202e36f67cab379768ac6be304c8ebde/src/tokens/ERC721.sol#L180) have versions of this function


#### Findings:
```
contracts/l2/token/L2GraphToken.sol:L81        _mint(_account, _amount);

contracts/l2/token/GraphTokenUpgradeable.sol:L133        _mint(_to, _amount);

contracts/l2/token/GraphTokenUpgradeable.sol:L155        _mint(_owner, _initialSupply);

```
