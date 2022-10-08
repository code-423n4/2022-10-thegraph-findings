## Unnecessary equals boolean


Boolean variables can be checked within conditionals directly without the use of equality operators to true/false.

### Code instances:

        GNS.sol, 717: require(legacySubgraphExists == true, "GNS: Subgraph does not exist");
        GNS.sol, 818: require(_isPublished(subgraphData) == true, "GNS: Must be active");
        GNS.sol, 506: require(subgraphData.disabled == true, "GNS: Must be disabled first");
        Staking.sol, 202: return msg.sender == _indexer || isOperator(msg.sender, _indexer) == true;
        ServiceRegistry.sol, 27: return msg.sender == _indexer || staking().isOperator(msg.sender, _indexer) == true;
        GNS.sol, 804: return _subgraphData.subgraphDeploymentID != 0 && _subgraphData.disabled == false;
        L1GraphTokenGateway.sol, 214: extraData.length == 0 || callhookWhitelist[msg.sender] == true,
        Staking.sol, 970: require(assetHolders[msg.sender] == true, "!assetHolder");



## Unused state variables

Unused state variables are gas consuming at deployment (since they are located in storage) and are
a bad code practice. Removing those variables will decrease deployment gas cost and improve code quality.
This is a full list of all the unused storage variables we found in your code base.

### Code instances:

        DisputeManagerStorage.sol, fishermanRewardPercentage
        L2ArbitrumMessenger.sol, ARB_SYS_ADDRESS
        EpochManagerStorage.sol, epochLength
        GNSStorage.sol, subgraphNFT
        RewardsManagerStorage.sol, tokenSupplySnapshot
        GraphGovernanceStorage.sol, proposals




## Unnecessary array boundaries check when loading an array element twice


    There are places in the code (especially in for-each loops) that loads the same array element more than once.
    In such cases, only one array boundaries check should take place, and the rest are unnecessary.
    Therefore, this array element should be cached in a local variable and then be loaded
    again using this local variable, skipping the redundant second array boundaries check:

### Code instance:

        Staking.sol.closeAllocationMany - double load of _requests[i]



## Caching array length can save gas


Caching the array length is more gas efficient.
This is because access to a local variable in solidity is more efficient than query storage / calldata / memory.
We recommend to change from:

    for (uint256 i=0; i<array.length; i++) { ... }

to:

    uint len = array.length
    for (uint256 i=0; i<len; i++) { ... }


### Code instances:

        Multicall.sol, data, 19
        Base58Encoder.sol, source, 18
        Base58Encoder.sol, indices, 53
        Staking.sol, _requests, 924
        RewardsManager.sol, _subgraphDeploymentID, 161
        AllocationExchange.sol, _vouchers, 136
        Base58Encoder.sol, input, 45
        Staking.sol, _allocationID, 1050



## Prefix increments are cheaper than postfix increments

Prefix increments are cheaper than postfix increments.
Further more, using unchecked {++x} is even more gas efficient, and the gas saving accumulates every iteration and can make a real change
There is no risk of overflow caused by increamenting the iteration index in for loops (the `++i` in `for (uint256 i = 0; i < numIterations; ++i)`).
But increments perform overflow checks that are not necessary in this case.
These functions use not using prefix increments (`++x`) or not using the unchecked keyword:

### Code instances:

        change to prefix increment and unchecked: RewardsManager.sol, i, 161
        change to prefix increment and unchecked: Base58Encoder.sol, i, 53
        change to prefix increment and unchecked: Staking.sol, i, 924
        change to prefix increment and unchecked: Base58Encoder.sol, i, 37
        change to prefix increment and unchecked: AllocationExchange.sol, i, 136
        change to prefix increment and unchecked: Multicall.sol, i, 19
        change to prefix increment and unchecked: Base58Encoder.sol, i, 45
        change to prefix increment and unchecked: Staking.sol, i, 1050
        just change to unchecked: Base58Encoder.sol, i, 18



## Unnecessary index init


In for loops you initialize the index to start from 0, but it already initialized to 0 in default and this assignment cost gas.
It is more clear and gas efficient to declare without assigning 0 and will have the same meaning:

### Code instances:

        AllocationExchange.sol, 136
        Staking.sol, 924
        Base58Encoder.sol, 37
        Base58Encoder.sol, 18
        Multicall.sol, 19
        Staking.sol, 1050
        Base58Encoder.sol, 45
        Base58Encoder.sol, 53
        RewardsManager.sol, 161



## Storage double reading. Could save SLOAD

Reading a storage variable is gas costly (SLOAD). In cases of multiple read of a storage variable in the same scope, caching the first read (i.e saving as a local variable) can save gas and decrease the
 overall gas uses. The following is a list of functions and the storage variables that you read twice:

### Code instances:

        Staking.sol: MAX_PPM is read twice in _setDelegationParameters
        Staking.sol: MAX_PPM is read twice in _stake
        GNS.sol: MAX_PPM is read twice in _setOwnerTaxPercentage
        LibFixedMath.sol: FIXED_1 is read twice in ln



## Unnecessary default assignment


Unnecessary default assignments, you can just declare and it will save gas and have the same meaning.


### Code instance:

        LibFixedMath.sol (L#41) : int256 private constant EXP_MAX_VAL = 0;



## Rearrange state variables

You can change the order of the storage variables to decrease memory uses.

### Code instances:

In Pausable.sol,rearranging the storage fields can optimize to: 3 slots from: 4 slots.
The new order of types (you choose the actual variables):
        1. uint256
        2. uint256
        3. address
        4. bool
        5. bool

In BancorFormula.sol,rearranging the storage fields can optimize to: 9 slots from: 10 slots.
The new order of types (you choose the actual variables):
        1. uint256
        2. uint256
        3. uint256
        4. uint256
        5. uint256
        6. uint256
        7. uint256
        8. uint256
        9. uint32
        10. uint16
        11. uint8
        12. uint8

In DisputeManagerStorage.sol,rearranging the storage fields can optimize to: 3 slots from: 4 slots.
The new order of types (you choose the actual variables):
        1. bytes32
        2. uint256
        3. address
        4. uint32
        5. uint32
        6. uint32




## Use bytes32 instead of string to save gas whenever possible


    Use bytes32 instead of string to save gas whenever possible.
    String is a dynamic data structure and therefore is more gas consuming then bytes32.


### Code instance:

        SubgraphNFT.sol (L153), string memory _subgraphURI = metadata > 0 ? HexStrings.toString(metadata) : "";



## Short the following require messages

The following require messages are of length more than 32 and we think are short enough to short
them into exactly 32 characters such that it will be placed in one slot of memory and the require
function will cost less gas.
The list:

### Code instances:

        Solidity file: Curation.sol, In line 195, Require message length to shorten: 35, The message: Caller must be the staking contract
        Solidity file: RewardsManager.sol, In line 95, Require message length to shorten: 35, The message: Issuance rate under minimum allowed
        Solidity file: GRTWithdrawHelper.sol, In line 63, Require message length to shorten: 33, The message: GRTWithdrawHelper: !returnAddress
        Solidity file: GNS.sol, In line 194, Require message length to shorten: 33, The message: Owner tax must be MAX_PPM or less
        Solidity file: GraphProxy.sol, In line 141, Require message length to shorten: 33, The message: Implementation must be a contract
        Solidity file: EthereumDIDRegistry.sol, In line 60, Require message length to shorten: 33, The message: Signer must be the identity owner


## Use != 0 instead of > 0


Using != 0 is slightly cheaper than > 0. (see https://github.com/code-423n4/2021-12-maple-findings/issues/75 for similar issue)


### Code instances:

        BancorFormula.sol, 194: change '_reserveRatio > 0' to '_reserveRatio != 0'
        BancorFormula.sol, 289: change '_toReserveRatio > 0' to '_toReserveRatio != 0'
        LibFixedMath.sol, 404: change 'b > 0' to 'b != 0'
        BancorFormula.sol, 234: change '_supply > 0' to '_supply != 0'
        Staking.sol, 1119: change '_tokens > 0' to '_tokens != 0'
        BancorFormula.sol, 285: change '_fromReserveBalance > 0' to '_fromReserveBalance != 0'


## Unnecessary cast



### Code instances:

        DisputeType DisputeManager.sol._slashIndexer - unnecessary casting DisputeType(_disputeType)
        uint256 Staking.sol._collectCurationFees - unnecessary casting uint256(_curationPercentage)
        uint256 Staking.sol._collectTax - unnecessary casting uint256(_percentage)



## Use unchecked to save gas for certain additive calculations that cannot overflow


You can use unchecked in the following calculations since there is no risk to overflow:

### Code instances:

        EthereumDIDRegistry.sol (L#235) - emit DIDAttributeChanged( identity, name, value, block.timestamp + validity, changed[identity] );
        EthereumDIDRegistry.sol (L#118) - delegates[identity][keccak256(abi.encode(delegateType))][delegate] = block.timestamp + validity;
        EthereumDIDRegistry.sol (L#126) - block.timestamp + validity, changed[identity] );



## Consider inline the following functions to save gas


    You can inline the following functions instead of writing a specific function to save gas.
    (see https://github.com/code-423n4/2021-11-nested-findings/issues/167 for a similar issue.)


### Code instances

        LibFixedMath.sol, toInteger, { return f / FIXED_1; }
        MathUtils.sol, min, { return x <= y ? x : y; }
        MathUtils.sol, diffOrZero, { return (x > y) ? x.sub(y) : 0; }
        DisputeManager.sol, _isDisputeInConflict, { return _dispute.relatedDisputeID != 0; }



## Inline one time use functions


The following functions are used exactly once. Therefore you can inline them and save gas and improve code clearness.


### Code instances:

        BancorFormula.sol, floorLog2
        RewardsManager.sol, _pow
        Stakes.sol, tokensWithdrawable
        GNS.sol, _chargeOwnerTax
        GraphTokenUpgradeable.sol, _getChainID
        Base58Encoder.sol, reverse
        Base58Encoder.sol, toAlphabet
        GNS.sol, _nextAccountSeqID
        Stakes.sol, allocate
        DisputeManager.sol, _slashIndexer
        DisputeManager.sol, _onlyArbitrator
        DisputeManager.sol, _getSlashingPercentageForDisputeType
        GraphUpgradeable.sol, _implementation
        BancorFormula.sol, findPositionInMaxExpArray
        SubgraphNFT.sol, _setMinter
        Staking.sol, _getEffectiveAllocation
        GraphProxyStorage.sol, _pendingImplementation
        Staking.sol, _undelegate
        GNS.sol, _nextSubgraphID
        Base58Encoder.sol, truncate
        Curation.sol, _tokensToSignal
        HexStrings.sol, toHexString
        DisputeManager.sol, _toUint8
        Staking.sol, _collectCurationFees
        Staking.sol, _collectDelegationIndexingRewards
        Managed.sol, _onlyController
        BancorFormula.sol, generalExp
        Stakes.sol, unlockTokens
        Staking.sol, _distributeRewards
        Stakes.sol, tokensUsed
        Stakes.sol, tokensAvailableWithDelegation
        L1GraphTokenGateway.sol, parseOutboundData
        BancorFormula.sol, optimalExp
        DisputeManager.sol, _createIndexingDisputeWithAllocation
        SubgraphNFT.sol, _setTokenDescriptor
        Staking.sol, _collectDelegationQueryRewards
        Managed.sol, _notPartialPaused
        Managed.sol, _notPaused
        DisputeManager.sol, _getChainID
        L2GraphTokenGateway.sol, parseOutboundData
        BancorFormula.sol, optimalLog
        Rebates.sol, unclaimedFees
        GraphProxyStorage.sol, _implementation
        BancorFormula.sol, generalLog
        Stakes.sol, release
        GNS.sol, _burnNFT
        GraphTokenUpgradeable.sol, _initialize
        RewardsManager.sol, _setIssuanceRate
        GraphToken.sol, _getChainID
        Managed.sol, _onlyGovernor
        DisputeManager.sol, _recoverAttestationSigner



## Change if -> revert pattern to require

Change if -> revert pattern to 'require' to save gas and improve code quality,
if (some_condition) {
        revert(revert_message)
}

to: require(!some_condition, revert_message)

In the following locations:

### Code instances:

        LibFixedMath.sol, 140
        LibFixedMath.sol, 153
        LibFixedMath.sol, 59
        LibFixedMath.sol, 386
        LibFixedMath.sol, 156
        LibFixedMath.sol, 88
        LibFixedMath.sol, 396
        LibFixedMath.sol, 405
        LibFixedMath.sol, 100
        LibFixedMath.sol, 267
        LibFixedMath.sol, 137
        LibFixedMath.sol, 393



## Upgrade pragma to at least 0.8.4


Using newer compiler versions and the optimizer gives gas optimizations
and additional safety checks are available for free.

The advantages of versions 0.8.* over <0.8.0 are:

        1. Safemath by default from 0.8.0 (can be more gas efficient than library based safemath.)
        2. Low level inliner : from 0.8.2, leads to cheaper runtime gas. Especially relevant when the contract has small functions. For example, OpenZeppelin libraries typically have a lot of small helper functions and if they are not inlined, they cost an additional 20 to 40 gas because of 2 extra jump instructions and additional stack operations needed for function calls.
        3. Optimizer improvements in packed structs: Before 0.8.3, storing packed structs, in some cases used an additional storage read operation. After EIP-2929, if the slot was already cold, this means unnecessary stack operations and extra deploy time costs. However, if the slot was already warm, this means additional cost of 100 gas alongside the same unnecessary stack operations and extra deploy time costs.
        4. Custom errors from 0.8.4, leads to cheaper deploy time cost and run time cost. Note: the run time cost is only relevant when the revert condition is met. In short, replace revert strings by custom errors.

### Code instances:

        Managed.sol
        TokenUtils.sol
        IServiceRegistry.sol
        IRewardsManager.sol
        ISubgraphNFT.sol




## Do not cache msg.sender


We recommend not to cache msg.sender since calling it is 2 gas while reading a variable is more.


### Code instances:

        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/discovery/GNS.sol#L325
        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/disputes/DisputeManager.sol#L403
        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/Staking.sol#L825
        https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/discovery/GNS.sol#L445

