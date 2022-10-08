## Use a newer version of Solidity
 # Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

==========================


## USE CUSTOM ERRORS INSTEAD OF REVERT STRINGS TO SAVE GAS 
 # Custom errors are more gas efficient than using require with a string explanation. So ideally you'd always use this over require.


There are 56 instances of this issue:
 ====================== 
### file:GraphGovernance.sol 
1.require(_governor != address(0), "governor != 0");
 2.require(_proposalId != 0x0, "!proposalId"); 
3.require(_votes != 0x0, "!votes");  

1.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/GraphGovernance.sol#L39 
2.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/GraphGovernance.sol#L69
3.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/GraphGovernance.sol#L70  
=========================== 

### file: L1GraphTokenGateway.sol   

4.require(inbox != address(0), "INBOX_NOT_SET"); 
5.require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");  

4.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L74
5.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L78  ========================= 

### file: AllocationExchange.sol  

6.require(_governor != address(0), "Exchange: governor must be set");
7.require(_to != address(0), "Exchange: empty destination");  

6.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/statechannels/AllocationExchange.sol#L65
7.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/statechannels/AllocationExchange.sol#L88  ========================== 

### file: L2GraphTokenGateway.sol   

8.require(msg.sender == AddressAliasHelper.applyL1ToL2Alias(l1Counterpart),
9. require(_l2Router != address(0), "INVALID_L2_ROUTER");  

8.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L69
9.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L98  =========================== 

### file: Curation.sol

  10.require(_bondingCurve != address(0), "Bonding curve must be set");
11.require(_defaultReserveRatio > 0, "Default reserve ratio must be > 0"); 

10.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/Curation.sol#L83
11.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/Curation.sol#L109   ======================= 
### file: Staking.sol  

12.require(slashers[msg.sender] == true, "!slasher");
13. require(_minimumIndexerStake > 0, "!minimumIndexerStake");  

12.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/Staking.sol#L194
13.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/Staking.sol#L254

========================
### file: Managed.sol

  14.require(!controller.paused(), "Paused");
15.require(!controller.partialPaused(), "Partial-paused"); 16. require(!controller.paused(), "Paused");  
16.require(!controller.paused(), "Paused");

14.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L44 
15.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L45 
16.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L49  

========================== 

### file: EpochManager.sol

  17.require(_epochLength > 0, "Epoch length cannot be 0"); 
18.require(_epochLength > 0, "Epoch length cannot be 0");  

17.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/EpochManager.sol#L28 
18.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/epochs/EpochManager.sol#L47

  ========================= 

### file:DisputeManager.sol  

19.require(msg.sender == arbitrator, "Caller is not the Arbitrator"); 
20.require(_arbitrator != address(0), "Arbitrator must be set")  

19.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/disputes/DisputeManager.sol#L146 
20.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/disputes/DisputeManager.sol#L211  

======================= 

### file:ServiceRegistry.so

l  21.require(_isAuth(_indexer), "!auth"); 
22.require(bytes(_url).length > 0, "Service must specify a URL");  

21.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/discovery/ServiceRegistry.sol#L71 
22.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/discovery/ServiceRegistry.sol#L72

  =====================

 ### file:GraphTokenUpgradeable.sol  

23.require(isMinter(msg.sender), "Only minter can call"); 
24.require(_owner == recoveredAddress, "GRT: invalid permit");

 23.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L60 
24.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L94  =========================

 ### file:GNS.sol  

25.require(ownerOf(_subgraphID) == msg.sender, "GNS: Must be authorized"); 
26.require(_ownerTaxPercentage <= MAX_PPM, "Owner tax must be MAX_PPM or less");  

25.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/discovery/GNS.sol#L146 26.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/discovery/GNS.sol#L194  ======================== 

### file: Controller.sol

  27.require(msg.sender == governor || msg.sender == pauseGuardian,  "Only Governor or Guardian can call”); 
28.require(_contractAddress != address(0), "Contract address must be set");  

27.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Controller.sol#L34 
28.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Controller.sol#L60  

======================= 
### file:Governed.sol  

29. require(msg.sender == governor, "Only Governor can call");
 30.require(_newGovernor != address(0), "Governor must be set");  

29.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L24 
30.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L41

  ====================== 
### file:GraphTokenGateway.sol

  31.require( msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,…) 
32.require(_newPauseGuardian != address(0), "PauseGuardian must be set");  

31.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L19
 32.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/GraphTokenGateway.sol#L31

  ====================== 

### file: L2GraphToken.sol

  33.require(msg.sender == gateway, "NOT_GATEWAY"); 
34.require(_owner != address(0), "Owner must be set");  


33.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L36 
34.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L49

  ===================== 

### file:GRTWithdrawHelper.sol 

 35.require(_wd.assetId == tokenAddress, "GRTWithdrawHelper: !token"); 
36.require(collectData.staking != address(0), "GRTWithdrawHelper: !staking");  

35.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/statechannels/GRTWithdrawHelper.sol#L57 
36.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/statechannels/GRTWithdrawHelper.sol#L61

  ===================== 

### file:GraphProxy.sol  

37. require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");  

37.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L105

  ==================== 

### file:SubgraphNFT.sol  

38.require(msg.sender == minter, "Must be a minter"); 
39.require( _tokenDescriptor == address(0) || Address.isContract(_tokenDescriptor),  

38.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/discovery/SubgraphNFT.sol#L30 
39.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/discovery/SubgraphNFT.sol#L73

  =================== 

### file: TokenUtils.sol  

40.require(_graphToken.transferFrom(_from, address(this), _amount), "!transfer"); 
41.require(_graphToken.transfer(_to, _amount), "!transfer");

  40.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/utils/TokenUtils.sol#L20 41.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/utils/TokenUtils.sol#L36  ==================== 

### file: GraphUpgradeable.sol  

42.require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
 43.require(msg.sender == _implementation(), "Caller must be the implementation");  

42.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L24 
43.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphUpgradeable.sol#L32  

=====================

 ### file:GraphToken.sol  

44. require(isMinter(msg.sender), "Only minter can call"); 
45. require(_owner == recoveredAddress, "GRT: invalid permit");  

44.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/GraphToken.sol#L56 45.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/token/GraphToken.sol#L117

  ====================== 

### file:RewardsManager.sol  

46.require( msg.sender == address(subgraphAvailabilityOracle), "Caller must be the subgraph availability oracle" 
47.require(_issuanceRate >= MIN_ISSUANCE_RATE, "Issuance rate under minimum allowed"); 

46.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/RewardsManager.sol#L62 
47.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/rewards/RewardsManager.sol#L95

  ========================= 

### file:BancorFormula.sol  

48.require( _supply > 0 && _reserveBalance > 0 && _reserveRatio > 0 && _reserveRatio <= MAX_RATI, …) 
49.require(_supply > 0 &&……..)  

48.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/bancor/BancorFormula.sol#L193 
49.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/bancor/BancorFormula.sol#L233

  ======================== 

### file:CallhookReceiverMock.sol  

50.require(foo != 0, "FOO_IS_ZERO");  

50.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/tests/CallhookReceiverMock.sol#L29  


======================= 

### file:HexStrings.sol  

51.require(value == 0, "Strings: hex length insufficient");  

51.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/libraries/HexStrings.sol#L33

  ====================== 

### file: EthereumDIDRegistry.sol

52.require(actor == identityOwner(identity), "Caller must be the identity owner");
 53.require(signer == identityOwner(identity), "Signer must be the identity owner");

  52.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/discovery/erc1056/EthereumDIDRegistry.sol#L22 53.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/discovery/erc1056/EthereumDIDRegistry.sol#L60  
======================== 

### file:L1ArbitrumMessenger.sol

  54.require(l2ToL1Sender != address(0), "NO_SENDER");  

54.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/arbitrum/L1ArbitrumMessenger.sol#L100

  ===================== 

### file:GraphProxyStorage.sol

  55.require(msg.sender == _admin(), "Caller must be admin");

  55.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L62  

====================  

### file:BridgeMock.sol  

56.require(outbox == msg.sender, "NOT_FROM_OUTBOX");  

56.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/tests/arbitrum/BridgeMock.sol#L61  

==================== 


