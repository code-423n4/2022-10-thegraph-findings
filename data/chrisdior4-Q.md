# Natspec errors 

There are 9 instances of this issue:

### file: L1ArbitrumMessenger.sol

1.functions can be easily overriden when testing  - overridden*
1.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/arbitrum/L1ArbitrumMessenger.sol#L32

### file: L2ArbitrumMessenger.sol
2..functions can be easily overriden when testing  - overridden*
2.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/arbitrum/L2ArbitrumMessenger.sol#L31


### file: Curation.sol
3.Staking contract and will do the bookeeping - bookkeeping*
3.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/Curation.sol#L188

4.Subgraph deployment curation poool - pool*
4.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/curation/Curation.sol#L360


### file: GNS.sol
5.The contract implements a multicall behaviour  - behavior
5.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/discovery/GNS.sol#L23

### file: DisputeManager.sol
6.Re-entrancy - Reentrancy*
6.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/disputes/DisputeManager.sol#L637

7.for incorrect behaviour. - behavior*
7.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/disputes/DisputeManager.sol#L19

### file: Staking.sol
8.private key for the allocationID adddress - address*
8.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/Staking.sol#L1113

9.Then we call collect() to do the transfer bookeeping - bookkeeping*
9.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/staking/Staking.sol#L1512

===========================

#Constants should be named in all caps

There are 2 instances of this issue:

### file: BancorFormula.sol

1.uint16 public constant version = 6;
1.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/bancor/BancorFormula.sol#L10

### file: GNS.sol
2.uint32 private constant defaultReserveRatio = 1000000;
2.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/discovery/GNS.sol#L37

==================

# EVENT IS MISSING INDEXED FIELDS
## Each event should use three indexed fields if there are three or more fields

There are 2instances of this issue:
 ================= 
### file: IBridge.sol


1. event MessageDelivered(
        uint256 indexed messageIndex,
        bytes32 indexed beforeInboxAcc,
        address inbox,
        uint8 kind,
        address sender,
        bytes32 messageDataHash
    );
1.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/arbitrum/IBridge.sol#L29

2.event BridgeCallTriggered(
        address indexed outbox,
        address indexed destAddr,
        uint256 amount,
        bytes data
    );
2.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/arbitrum/IBridge.sol#L38

==================

### file: IOutbox.sol
3.    event OutboxEntryCreated(
        uint256 indexed batchNum,
        uint256 outboxEntryIndex,
        bytes32 outputRoot,
        uint256 numInBatch
3.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/arbitrum/IOutbox.sol#L29

==================

# Require() should be used instead of assert()
Prior to solidity version 0.8.0, hitting an assert consumes the remainder of the transaction’s available gas rather than returning it, as require()/revert() do. assert() should be avoided even past solidity version 0.8.0 as its documentation states that “The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix”.use require/revert


### file: GraphProxy.sol

1.assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
2.assert(IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)
        );
3.assert(PENDING_IMPLEMENTATION_SLOT ==
                bytes32(uint256(keccak256("eip1967.proxy.pendingImplementation")) - 1)

1.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L47
2.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L49
3.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L51

================================

## THE NONREENTRANT MODIFIER SHOULD OCCUR BEFORE ALL OTHER MODIFIERS

### file: L2GraphTokenGateway.sol

1.public payable override notPaused nonReentrant returns (bytes memory) {
1.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L144

2.external payable override notPaused onlyL1Counterpart nonReentrant {
2.https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L232