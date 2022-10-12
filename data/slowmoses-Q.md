## Solidity Version

Consider using the most recent version of solidity.
0.8.17 is already released and most contracts in the project specify ^0.7.6.

Consider using the same version of solidity in all files.
A fixed version of solidity should be specified in all non-library files.

***

Most contracts in the project specify ^0.7.6.

pragma solidity ^0.7.6;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L3

But not all:

pragma solidity >=0.6.12 <0.8.0;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L3

pragma solidity >=0.6.12 <0.8.0;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStakingData.sol#L3

pragma solidity >=0.6.12 <0.8.0;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L3

## Sensitive Terms

Avoid the use of sensitive terms in favor of neutral ones. Use allowlist rather than whitelist.

***

    * Note that whitelisted senders (some protocol contracts) can include additional calldata
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L214

    * never succeeds. This requires extra care when adding contracts to the whitelist, but is necessary to ensure that
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L217

    * @param _data Extra callhook data, only used when the sender is whitelisted
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L224

    mapping(address => bool) public callhookWhitelist;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L35

    // Emitted when an address is added to the callhook whitelist
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L63

    event AddedToCallhookWhitelist(address newWhitelisted);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L64

    // Emitted when an address is removed from the callhook whitelist
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L65

    event RemovedFromCallhookWhitelist(address notWhitelisted);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L66

    * - whitelisted callhook callers using addToCallhookWhitelist
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L95

    * @dev Adds an address to the callhook whitelist.
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L148

    * @param _newWhitelisted Address to add to the whitelist
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L150

    function addToCallhookWhitelist(address _newWhitelisted) external onlyGovernor {
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L152

    require(_newWhitelisted != address(0), "INVALID_ADDRESS");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L153

    require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L154

    callhookWhitelist[_newWhitelisted] = true;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L155

    emit AddedToCallhookWhitelist(_newWhitelisted);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L156

    * @dev Removes an address from the callhook whitelist.
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L160

    * @param _notWhitelisted Address to remove from the whitelist
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L162

    function removeFromCallhookWhitelist(address _notWhitelisted) external onlyGovernor {
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L164

    require(_notWhitelisted != address(0), "INVALID_ADDRESS");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L165

    require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L166

    callhookWhitelist[_notWhitelisted] = false;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L167

    emit RemovedFromCallhookWhitelist(_notWhitelisted);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L168

    * Also note that whitelisted senders (some protocol contracts) can include additional calldata
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L177

    * never succeeds. This requires extra care when adding contracts to the whitelist, but is necessary to ensure that
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L180

    extraData.length == 0 || callhookWhitelist[msg.sender] == true,
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L214

    * @param _data Additional call data for the L2 transaction, which must be empty unless the caller is whitelisted
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L323

    * be whitelisted by the governor, but also implement this interface that contains
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/ICallhookReceiver.sol#L6

## Revert and Require Should Have Descriptive Reason Strings

require(success);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L34

require(success);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L47

require(success);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L59

require(success);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L133

revert(ptr, size)
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L179

## Missing NatSpec

Every function should have a short textual description.

In addition to the textual description, there should be a formal documentation for every parameter (@param).

There are A LOT of instances of this problem listed below. Quite a few borderline ok instances (missing formal parameters but described as text or obvious parameters) were not included in the report.

***

Undocumented parameter: data
    
    /**
    * @dev Admin function for new implementation to accept its role as implementation.
    */
    function acceptUpgradeAndCall(bytes calldata data) external ifAdminOrPendingImpl {
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L125-L129

***

Undocumented function:

    function _notPartialPaused() internal view {
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L43-L43

***

Undocumented function:

    function _notPaused() internal view virtual {
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L48-L48

***

Undocumented function:

    function _onlyGovernor() internal view {
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L52-L52

***

Undocumented function:

    function _onlyController() internal view {
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L56-L56

***

Undocumented parameter: calldata
    
    /**
    * @notice Receives withdrawn tokens from L2
    * The equivalent tokens are released from escrow and sent to the destination.
    * @dev can only accept transactions coming from the L2 GRT Gateway.
    * The last parameter is unused but kept for compatibility with Arbitrum gateways,
    * and the encoded exitNum is assumed to be 0.
    * @param _l1Token L1 Address of the GRT contract (needed for compatibility with Arbitrum Gateway Router)
    * @param _from Address of the sender
    * @param _to Recepient address on L1
    * @param _amount Amount of tokens transferred
    */
    function finalizeInboundTransfer(
    address _l1Token,
    address _from,
    address _to,
    uint256 _amount,
    bytes calldata // _data, contains exitNum, unused by this contract
    ) external payable override notPaused onlyL2Counterpart {
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L251-L269

***

Undocumented function:

    function initialize(address _owner) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/IGraphCurationToken.sol#L8-L8

***

Undocumented function:

    function burnFrom(address _account, uint256 _amount) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/IGraphCurationToken.sol#L10-L10

***

Undocumented function:

    function mint(address _to, uint256 _amount) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/IGraphCurationToken.sol#L12-L12

***

Undocumented function:

    function admin() external returns (address);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/IGraphProxy.sol#L6-L6

***

Undocumented function:

    function setAdmin(address _newAdmin) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/IGraphProxy.sol#L8-L8

***

Undocumented function:

    function implementation() external returns (address);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/IGraphProxy.sol#L10-L10

***

Undocumented function:

    function pendingImplementation() external returns (address);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/IGraphProxy.sol#L12-L12

***

Undocumented function:

    function upgradeTo(address _newImplementation) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/IGraphProxy.sol#L14-L14

***

Undocumented function:

    function acceptUpgrade() external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/IGraphProxy.sol#L16-L16

***

Undocumented function:

    function acceptUpgradeAndCall(bytes calldata data) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/IGraphProxy.sol#L18-L18

***

Undocumented function:

    function setEpochLength(uint256 _epochLength) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L8-L8

***

Undocumented function:

    function runEpoch() external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L12-L12

***

Undocumented function:

    function isCurrentEpochRun() external view returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L16-L16

***

Undocumented function:

    function blockNum() external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L18-L18

***

Undocumented function:

    function blockHash(uint256 _block) external view returns (bytes32);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L20-L20

***

Undocumented function:

    function currentEpoch() external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L22-L22

***

Undocumented function:

    function currentEpochBlock() external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L24-L24

***

Undocumented function:

    function currentEpochBlockSinceStart() external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L26-L26

***

Undocumented function:

    function epochsSince(uint256 _epoch) external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L28-L28

***

Undocumented function:

    function epochsSinceUpdate() external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/epochs/IEpochManager.sol#L30-L30

***

Undocumented function:

    function getGovernor() external view returns (address);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L6-L6

***

Undocumented function:

    function setContractProxy(bytes32 _id, address _contractAddress) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L10-L10

***

Undocumented function:

    function unsetContractProxy(bytes32 _id) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L12-L12

***

Undocumented function:

    function updateController(bytes32 _id, address _controller) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L14-L14

***

Undocumented function:

    function getContractProxy(bytes32 _id) external view returns (address);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L16-L16

***

Undocumented function:

    function setPartialPaused(bool _partialPaused) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L20-L20

***

Undocumented function:

    function setPaused(bool _paused) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L22-L22

***

Undocumented function:

    function setPauseGuardian(address _newPauseGuardian) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L24-L24

***

Undocumented function:

    function paused() external view returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L26-L26

***

Undocumented function:

    function partialPaused() external view returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/IController.sol#L28-L28

***

Undocumented function:

    function burn(uint256 amount) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L10-L10

***

Undocumented function:

    function burnFrom(address _from, uint256 amount) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L12-L12

***

Undocumented function:

    function mint(address _to, uint256 _amount) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L14-L14

***

Undocumented function:

    function addMinter(address _account) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L18-L18

***

Undocumented function:

    function removeMinter(address _account) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L20-L20

***

Undocumented function:

    function renounceMinter() external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L22-L22

***

Undocumented function:

    function isMinter(address _account) external view returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L24-L24

***

Undocumented function:

    function permit(
    address _owner,
    address _spender,
    uint256 _value,
    uint256 _deadline,
    uint8 _v,
    bytes32 _r,
    bytes32 _s
    ) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L28-L36

***

Undocumented function:

    function increaseAllowance(address spender, uint256 addedValue) external returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L40-L40

***

Undocumented function:

    function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/token/IGraphToken.sol#L42-L42

***

Undocumented function:

    function setIssuanceRate(uint256 _issuanceRate) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L18-L18

***

Undocumented function:

    function setMinimumSubgraphSignal(uint256 _minimumSubgraphSignal) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L20-L20

***

Undocumented function:

    function setSubgraphAvailabilityOracle(address _subgraphAvailabilityOracle) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L24-L24

***

Undocumented function:

    function setDenied(bytes32 _subgraphDeploymentID, bool _deny) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L26-L26

***

Undocumented function:

    function setDeniedMany(bytes32[] calldata _subgraphDeploymentID, bool[] calldata _deny)
    external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L28-L29

***

Undocumented function:

    function isDenied(bytes32 _subgraphDeploymentID) external view returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L31-L31

***

Undocumented function:

    function getNewRewardsPerSignal() external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L35-L35

***

Undocumented function:

    function getAccRewardsPerSignal() external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L37-L37

***

Undocumented function:

    function getAccRewardsForSubgraph(bytes32 _subgraphDeploymentID)
    external
    view
    returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L39-L42

***

Undocumented function:

    function getAccRewardsPerAllocatedToken(bytes32 _subgraphDeploymentID)
    external
    view
    returns (uint256, uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L44-L47

***

Undocumented function:

    function getRewards(address _allocationID) external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L49-L49

***

Undocumented function:

    function updateAccRewardsPerSignal() external returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L53-L53

***

Undocumented function:

    function takeRewards(address _allocationID) external returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L55-L55

***

Undocumented function:

    function onSubgraphSignalUpdate(bytes32 _subgraphDeploymentID) external returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L59-L59

***

Undocumented function:

    function onSubgraphAllocationUpdate(bytes32 _subgraphDeploymentID) external returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/rewards/IRewardsManager.sol#L61-L61

***

Undocumented function:

    function setDefaultReserveRatio(uint32 _defaultReserveRatio) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L10-L10

***

Undocumented function:

    function setMinimumCurationDeposit(uint256 _minimumCurationDeposit) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L12-L12

***

Undocumented function:

    function setCurationTaxPercentage(uint32 _percentage) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L14-L14

***

Undocumented function:

    function setCurationTokenMaster(address _curationTokenMaster) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L16-L16

***

Undocumented function:

    function mint(
    bytes32 _subgraphDeploymentID,
    uint256 _tokensIn,
    uint256 _signalOutMin
    ) external returns (uint256, uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L20-L24

***

Undocumented function:

    function burn(
    bytes32 _subgraphDeploymentID,
    uint256 _signalIn,
    uint256 _tokensOutMin
    ) external returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L26-L30

***

Undocumented function:

    function collect(bytes32 _subgraphDeploymentID, uint256 _tokens) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L32-L32

***

Undocumented function:

    function isCurated(bytes32 _subgraphDeploymentID) external view returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L36-L36

***

Undocumented function:

    function getCuratorSignal(address _curator, bytes32 _subgraphDeploymentID)
    external
    view
    returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L38-L41

***

Undocumented function:

    function getCurationPoolSignal(bytes32 _subgraphDeploymentID) external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L43-L43

***

Undocumented function:

    function getCurationPoolTokens(bytes32 _subgraphDeploymentID) external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L45-L45

***

Undocumented function:

    function tokensToSignal(bytes32 _subgraphDeploymentID, uint256 _tokensIn)
    external
    view
    returns (uint256, uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L47-L50

***

Undocumented function:

    function signalToTokens(bytes32 _subgraphDeploymentID, uint256 _signalIn)
    external
    view
    returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L52-L55

***

Undocumented function:

    function curationTaxPercentage() external view returns (uint32);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/curation/ICuration.sol#L57-L57

***

Undocumented function:

    function setMinimumIndexerStake(uint256 _minimumIndexerStake) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L30-L30

***

Undocumented function:

    function setThawingPeriod(uint32 _thawingPeriod) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L32-L32

***

Undocumented function:

    function setCurationPercentage(uint32 _percentage) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L34-L34

***

Undocumented function:

    function setProtocolPercentage(uint32 _percentage) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L36-L36

***

Undocumented function:

    function setChannelDisputeEpochs(uint32 _channelDisputeEpochs) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L38-L38

***

Undocumented function:

    function setMaxAllocationEpochs(uint32 _maxAllocationEpochs) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L40-L40

***

Undocumented function:

    function setRebateRatio(uint32 _alphaNumerator, uint32 _alphaDenominator) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L42-L42

***

Undocumented function:

    function setDelegationRatio(uint32 _delegationRatio) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L44-L44

***

Undocumented function:

    function setDelegationParameters(
    uint32 _indexingRewardCut,
    uint32 _queryFeeCut,
    uint32 _cooldownBlocks
    ) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L46-L50

***

Undocumented function:

    function setDelegationParametersCooldown(uint32 _blocks) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L52-L52

***

Undocumented function:

    function setDelegationUnbondingPeriod(uint32 _delegationUnbondingPeriod) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L54-L54

***

Undocumented function:

    function setDelegationTaxPercentage(uint32 _percentage) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L56-L56

***

Undocumented function:

    function setSlasher(address _slasher, bool _allowed) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L58-L58

***

Undocumented function:

    function setAssetHolder(address _assetHolder, bool _allowed) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L60-L60

***

Undocumented function:

    function setOperator(address _operator, bool _allowed) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L64-L64

***

Undocumented function:

    function isOperator(address _operator, address _indexer) external view returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L66-L66

***

Undocumented function:

    function stake(uint256 _tokens) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L70-L70

***

Undocumented function:

    function stakeTo(address _indexer, uint256 _tokens) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L72-L72

***

Undocumented function:

    function unstake(uint256 _tokens) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L74-L74

***

Undocumented function:

    function slash(
    address _indexer,
    uint256 _tokens,
    uint256 _reward,
    address _beneficiary
    ) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L76-L81

***

Undocumented function:

    function withdraw() external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L83-L83

***

Undocumented function:

    function setRewardsDestination(address _destination) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L85-L85

***

Undocumented function:

    function delegate(address _indexer, uint256 _tokens) external returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L89-L89

***

Undocumented function:

    function undelegate(address _indexer, uint256 _shares) external returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L91-L91

***

Undocumented function:

    function withdrawDelegated(address _indexer, address _newIndexer) external returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L93-L93

***

Undocumented function:

    function allocate(
    bytes32 _subgraphDeploymentID,
    uint256 _tokens,
    address _allocationID,
    bytes32 _metadata,
    bytes calldata _proof
    ) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L97-L103

***

Undocumented function:

    function allocateFrom(
    address _indexer,
    bytes32 _subgraphDeploymentID,
    uint256 _tokens,
    address _allocationID,
    bytes32 _metadata,
    bytes calldata _proof
    ) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L105-L112

***

Undocumented function:

    function closeAllocation(address _allocationID, bytes32 _poi) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L114-L114

***

Undocumented function:

    function closeAllocationMany(CloseAllocationRequest[] calldata _requests) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L116-L116

***

Undocumented function:

    function closeAndAllocate(
    address _oldAllocationID,
    bytes32 _poi,
    address _indexer,
    bytes32 _subgraphDeploymentID,
    uint256 _tokens,
    address _allocationID,
    bytes32 _metadata,
    bytes calldata _proof
    ) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L118-L127

***

Undocumented function:

    function collect(uint256 _tokens, address _allocationID) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L129-L129

***

Undocumented function:

    function claim(address _allocationID, bool _restake) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L131-L131

***

Undocumented function:

    function claimMany(address[] calldata _allocationID, bool _restake) external;
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L133-L133

***

Undocumented function:

    function hasStake(address _indexer) external view returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L137-L137

***

Undocumented function:

    function getIndexerStakedTokens(address _indexer) external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L139-L139

***

Undocumented function:

    function getIndexerCapacity(address _indexer) external view returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L141-L141

***

Undocumented function:

    function getAllocation(address _allocationID) external view returns (Allocation memory);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L143-L143

***

Undocumented function:

    function getAllocationState(address _allocationID) external view returns (AllocationState);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L145-L145

***

Undocumented function:

    function isAllocation(address _allocationID) external view returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L147-L147

***

Undocumented function:

    function getSubgraphAllocatedTokens(bytes32 _subgraphDeploymentID)
    external
    view
    returns (uint256);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L149-L152

***

Undocumented function:

    function getDelegation(address _indexer, address _delegator)
    external
    view
    returns (Delegation memory);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L154-L157

***

Undocumented function:

    function isDelegator(address _indexer, address _delegator) external view returns (bool);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStaking.sol#L159-L159



