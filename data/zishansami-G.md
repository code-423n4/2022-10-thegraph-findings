## Gas Optimizations
 *** 


### G-01: No need to compare boolean expressions with boolean literals, directly use the expression
if (<x> == true) ==> if (<x>)
if (<x> == false) => if (!<x>)

Total instances of this issue: 1

instance #1
Permalink: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L213-L216
```solidity
contracts/gateway/L1GraphTokenGateway.sol

213:                require(
214:                    extraData.length == 0 || callhookWhitelist[msg.sender] == true,
215:                    "CALL_HOOK_DATA_NOT_ALLOWED"
216:                );

```

 *** 


### G-02: Adding `payable` to functions which are only meant to be called by specific actors like `onlyOwner` will save gas cost
Marking functions payable removes additional checks for whether a payment was provided, hence reducing gas cost

Total instances of this issue: 2

instance #1
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L56
```solidity
contracts/governance/Managed.sol

56:    function _onlyController() internal view {

```

instance #2
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L95
```solidity
contracts/governance/Managed.sol

95:    function setController(address _controller) external onlyController {

```

 *** 


### G-03: `require()` containing multiple checks joined with && should be broken into multiple require statements

Total instances of this issue: 3

instance #1
Permalink: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L54-L57
```solidity
contracts/governance/Governed.sol

54:        require(
55:            pendingGovernor != address(0) && msg.sender == pendingGovernor,
56:            "Caller must be pending governor"
57:        );

```

instance #2
Permalink: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L142-L145
```solidity
contracts/upgrades/GraphProxy.sol

142:        require(
143:            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
144:            "Caller must be the pending implementation"
145:        );

```

instance #3
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142
```solidity
contracts/gateway/L1GraphTokenGateway.sol

142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

```

 *** 


### G-04: Using uints/ints smaller than 256 bits increases overhead
Gas usage becomes higher with uint/int smaller than 256 bits because EVM operates on 32 bytes and uses additional operations to reduce the size from 32 bytes to the target size.

Total instances of this issue: 15

instance #1
Permalink: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L74-L82
```solidity
contracts/l2/token/GraphTokenUpgradeable.sol

74:    function permit(
75:        address _owner,
76:        address _spender,
77:        uint256 _value,
78:        uint256 _deadline,
79:        uint8 _v,
80:        bytes32 _r,
81:        bytes32 _s
82:    ) external {

```

instance #2
Permalink: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/token/IGraphToken.sol#L28-L36
```solidity
contracts/token/IGraphToken.sol

28:    function permit(
29:        address _owner,
30:        address _spender,
31:        uint256 _value,
32:        uint256 _deadline,
33:        uint8 _v,
34:        bytes32 _r,
35:        bytes32 _s
36:    ) external;

```

instance #3
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol#L10
```solidity
contracts/curation/ICuration.sol

10:    function setDefaultReserveRatio(uint32 _defaultReserveRatio) external;

```

instance #4
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/curation/ICuration.sol#L14
```solidity
contracts/curation/ICuration.sol

14:    function setCurationTaxPercentage(uint32 _percentage) external;

```

instance #5
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L32
```solidity
contracts/staking/IStaking.sol

32:    function setThawingPeriod(uint32 _thawingPeriod) external;

```

instance #6
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L34
```solidity
contracts/staking/IStaking.sol

34:    function setCurationPercentage(uint32 _percentage) external;

```

instance #7
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L36
```solidity
contracts/staking/IStaking.sol

36:    function setProtocolPercentage(uint32 _percentage) external;

```

instance #8
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L38
```solidity
contracts/staking/IStaking.sol

38:    function setChannelDisputeEpochs(uint32 _channelDisputeEpochs) external;

```

instance #9
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L40
```solidity
contracts/staking/IStaking.sol

40:    function setMaxAllocationEpochs(uint32 _maxAllocationEpochs) external;

```

instance #10
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L42
```solidity
contracts/staking/IStaking.sol

42:    function setRebateRatio(uint32 _alphaNumerator, uint32 _alphaDenominator) external;

```

instance #11
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L44
```solidity
contracts/staking/IStaking.sol

44:    function setDelegationRatio(uint32 _delegationRatio) external;

```

instance #12
Permalink: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L46-L50
```solidity
contracts/staking/IStaking.sol

46:    function setDelegationParameters(
47:        uint32 _indexingRewardCut,
48:        uint32 _queryFeeCut,
49:        uint32 _cooldownBlocks
50:    ) external;

```

instance #13
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L52
```solidity
contracts/staking/IStaking.sol

52:    function setDelegationParametersCooldown(uint32 _blocks) external;

```

instance #14
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L54
```solidity
contracts/staking/IStaking.sol

54:    function setDelegationUnbondingPeriod(uint32 _delegationUnbondingPeriod) external;

```

instance #15
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/staking/IStaking.sol#L56
```solidity
contracts/staking/IStaking.sol

56:    function setDelegationTaxPercentage(uint32 _percentage) external;

```

 *** 


### G-05: Using `!= 0` costs less gas than `> 0` in a require() statement
Saves 6 gas per instance for solidity version less than 0.8.12.

Total instances of this issue: 3

instance #1
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L146
```solidity
contracts/l2/gateway/L2GraphTokenGateway.sol

146:        require(_amount > 0, "INVALID_ZERO_AMOUNT");

```

instance #2
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L201
```solidity
contracts/gateway/L1GraphTokenGateway.sol

201:        require(_amount > 0, "INVALID_ZERO_AMOUNT");

```

instance #3
Link: https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L217
```solidity
contracts/gateway/L1GraphTokenGateway.sol

217:                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

```

