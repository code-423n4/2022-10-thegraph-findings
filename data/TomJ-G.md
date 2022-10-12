## Table of Contents
Total of 11 issues found.
- Minimize the Number of SLOADs by Caching State Variable
- Defined Variables Used Only Once
- Redundant Use of Variable
- Duplicate require() Checks Should be a Modifier or a Function
- Use require instead of &&
- Internal Function Called Only Once can be Inlined
- Boolean Comparisons
- Using Elements Smaller than 32 bytes (256 bits) Might Use More Gas
- != 0 costs less gas than > 0
- Keep The Revert Strings of Error Messages within 32 Bytes
- Use Custom Errors to Save Gas

&ensp;
### Minimize the Number of SLOADs by Caching State Variable

#### Issue
SLOADs cost 100 gas where MLOADs/MSTOREs cost only 3 gas.
Whenever function reads storage value more than once, it should be cached to save gas.

#### PoC
Total of 3 instances found.

1. Cache `l1GRT` of outboundTransfer() of L2GraphTokenGateway.sol
2 SLOADs to 1 SLOAD, 1 MSTORE and 2 MLOAD
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L145
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L156

2. Cache `l1GRT` of finalizeInboundTransfer() of L2GraphTokenGateway.sol
2 SLOADs to 1 SLOAD, 1 MSTORE and 2 MLOAD
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L233
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/gateway/L2GraphTokenGateway.sol#L236

3. Cache `escrow` of finalizeInboundTransfer() of L1GraphTokenGateway.sol
2 SLOADs to 1 SLOAD, 1 MSTORE and 2 MLOAD
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L273
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L276

#### Mitigation
When certain state variable is read more than once, cache it to local variable to save gas.

&ensp;
### Defined Variables Used Only Once

#### Issue
Certain variables is defined even though they are used only once.
Remove these unnecessary variables to save gas.
For cases where it will reduce the readability, one can use comments to help describe
what the code is doing.

#### PoC
Total of 1 instance found.

1. Remove `escrowBalance` variable of finalizeInboundTransfer function of L1GraphTokenGateway.sol
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L273-L275
Mitigation:
Delete line 273 and replace line 275 with below code 
```solidity
        require(_amount <= token.balanceOf(escrow), "BRIDGE_OUT_OF_FUNDS");
```

#### Mitigation
Don't define variable that is used only once.
Details are listed on above PoC.

&ensp;
### Redundant Use of Variable

#### Issue
Below has redundant use of variables. 
Instead of defining a new variable, emit the event first and then update the variable.

#### PoC
Total of 4 instances found.

1. Do not define `oldPendingGovernor` of transferOwnership function of Governed.sol
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L40-L47
Change it to
```solidity
    function transferOwnership(address _newGovernor) external onlyGovernor {
        require(_newGovernor != address(0), "Governor must be set");

        emit NewPendingOwnership(pendingGovernor, _newGovernor);

        pendingGovernor = _newGovernor;
    }
```

2. Do not define `oldGovernor` and `oldPendingGovernor` of acceptOwnership function of Governed.sol
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L53-L67
Change it to
```solidity
    function acceptOwnership() external {
        require(
            pendingGovernor != address(0) && msg.sender == pendingGovernor,
            "Caller must be pending governor"
        );

        emit NewOwnership(governor, pendingGovernor);
        emit NewPendingOwnership(pendingGovernor, address(0));

        governor = pendingGovernor;
        pendingGovernor = address(0);
    }
```

3. Do not define `oldPauseGuardian` of _setPauseGuardian function of Pausable.sol
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L55-L59
Change it to
```solidity
    function _setPauseGuardian(address newPauseGuardian) internal {
        emit NewPauseGuardian(pauseGuardian, newPauseGuardian);
        pauseGuardian = newPauseGuardian;
    }
```

#### Mitigation
Instead of defining a new variable, emit the event first and then update the variable like shown in above PoC.

&ensp;
### Duplicate require() Checks Should be a Modifier or a Function

#### Issue
Since below require checks are used more than once,
I recommend making these to a modifier or a function.

#### PoC
Total of 5 instances found.
```solidity
./GraphProxyAdmin.sol:34:        require(success);
./GraphProxyAdmin.sol:47:        require(success);
./GraphProxyAdmin.sol:59:        require(success);
```
```solidity
./L1GraphTokenGateway.sol:200:        require(_l1Token == address(token), "TOKEN_NOT_GRT");
./L1GraphTokenGateway.sol:271:        require(_l1Token == address(token), "TOKEN_NOT_GRT");
```
```solidity
./L2GraphTokenGateway.sol:145:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
./L2GraphTokenGateway.sol:233:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
```
```solidity
./L2GraphTokenGateway.sol:147:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
./L2GraphTokenGateway.sol:234:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
```
```solidity
./Managed.sol:44:        require(!controller.paused(), "Paused");
./Managed.sol:49:        require(!controller.paused(), "Paused");
```

#### Mitigation
I recommend making duplicate require statement into modifier or a function.

&ensp;
### Use require instead of &&

#### Issue
When there are multiple conditions in require statement, break down the require statement into
multiple require statements instead of using && can save gas.

#### PoC
Total of 3 instances found.
```
./Governed.sol:54:        require(
                              pendingGovernor != address(0) && msg.sender == pendingGovernor,
                              "Caller must be pending governor"
                          );
./GraphProxy.sol:142:        require(
                                 _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
                                 "Caller must be the pending implementation"
                             );
./L1GraphTokenGateway.sol:142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```

#### Mitigation
Break down into several require statement.

&ensp;
### Internal Function Called Only Once Can be Inlined

#### Issue
Certain function is defined even though it is called only once.
Inline it instead to where it is called to avoid usage of extra gas.

#### PoC
Total of 3 instances found.

1. _notPartialPaused() of Managed.sol
_getCurrentDelegated function called only once at line 61
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L43
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L61

2. _onlyGovernor() of Managed.sol
_getCurrentDelegated function called only once at line 78
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L52
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L78

3. _onlyController() of Managed.sol
_getCurrentDelegated function called only once at line 72
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L56
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Managed.sol#L72

#### Mitigation
I recommend to not define above functions and instead inline it at place it is called.

&ensp;
### Boolean Comparisons

#### Issue
It is more gas expensive to compare boolean with "variable == true" or "variable == false" than 
directly checking the returned boolean value.

#### PoC
Total of 1 instance found.
```
./L1GraphTokenGateway.sol:214:                    extraData.length == 0 || callhookWhitelist[msg.sender] == true,
```

#### Mitigation
Simply check by returned boolean value.
Change it to
```
require(extraData.length == 0 || callhookWhitelist[msg.sender])
```

&ensp;
### Using Elements Smaller than 32 bytes (256 bits) Might Use More Gas

#### Issue
Since EVM operates on 32 bytes at a time, if the element is smaller than that, the EVM must use more operations
in order to reduce the elements from 32 bytes to specified size. Therefore it is more gas saving to use 32 bytes 
unless it is used in a struct or state variable in order to reduce storage slot. 

Reference: https://docs.soliditylang.org/en/v0.8.15/internals/layout_in_storage.html

#### PoC
Total of 18 instances found.
```
./GraphTokenUpgradeable.sol:79:        uint8 _v,
./ICuration.sol:10:    function setDefaultReserveRatio(uint32 _defaultReserveRatio) external;
./ICuration.sol:14:    function setCurationTaxPercentage(uint32 _percentage) external;
./ICuration.sol:57:    function curationTaxPercentage() external view returns (uint32);
./IGraphToken.sol:33:        uint8 _v,
./IStaking.sol:32:    function setThawingPeriod(uint32 _thawingPeriod) external;
./IStaking.sol:34:    function setCurationPercentage(uint32 _percentage) external;
./IStaking.sol:36:    function setProtocolPercentage(uint32 _percentage) external;
./IStaking.sol:38:    function setChannelDisputeEpochs(uint32 _channelDisputeEpochs) external;
./IStaking.sol:40:    function setMaxAllocationEpochs(uint32 _maxAllocationEpochs) external;
./IStaking.sol:42:    function setRebateRatio(uint32 _alphaNumerator, uint32 _alphaDenominator) external;
./IStaking.sol:44:    function setDelegationRatio(uint32 _delegationRatio) external;
./IStaking.sol:47:        uint32 _indexingRewardCut,
./IStaking.sol:48:        uint32 _queryFeeCut,
./IStaking.sol:49:        uint32 _cooldownBlocks
./IStaking.sol:52:    function setDelegationParametersCooldown(uint32 _blocks) external;
./IStaking.sol:54:    function setDelegationUnbondingPeriod(uint32 _delegationUnbondingPeriod) external;
./IStaking.sol:56:    function setDelegationTaxPercentage(uint32 _percentage) external;
```

#### Mitigation
I suggest using uint256 instead of anything smaller and downcast where needed.

&ensp;
### != 0 costs less gas than > 0

#### Issue
!= 0 costs less gas when optimizer is enabled and is used for unsigned integers in require statement.

#### PoC
Total of 3 instances found.
```
./L1GraphTokenGateway.sol:201:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
./L1GraphTokenGateway.sol:217:                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
./L2GraphTokenGateway.sol:146:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

#### Mitigation
I suggest changing > 0 to != 0

&ensp;
### Keep The Revert Strings of Error Messages within 32 Bytes

#### Issue
Since each storage slot is size of 32 bytes, error messages that is longer than this will need
extra storage slot leading to more gas cost.

#### PoC
Total of 6 instances found.
```
./GraphProxy.sol:105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
./GraphProxy.sol:141:        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
./GraphProxy.sol:142:        require(
                                 _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
                                 "Caller must be the pending implementation"
                             );
./GraphUpgradeable.sol:32:        require(msg.sender == _implementation(), "Caller must be the implementation");
./GraphTokenGateway.sol:19:        require(
                                       msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
                                       "Only Governor or Guardian can call"
                                   );
./Managed.sol:53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```

#### Mitigation
Simply keep the error messages within 32 bytes to avoid extra storage slot cost.

&ensp;
### Use Custom Errors to Save Gas

#### Issue
Custom errors from Solidity 0.8.4 are cheaper than revert strings.
Details are explained here: https://blog.soliditylang.org/2021/04/21/custom-errors/

#### PoC
Total of 64 instances found.
```
./Governed.sol:24:        require(msg.sender == governor, "Only Governor can call");
./Governed.sol:41:        require(_newGovernor != address(0), "Governor must be set");
./Governed.sol:54:        require(
                              pendingGovernor != address(0) && msg.sender == pendingGovernor,
                              "Caller must be pending governor"
                          );
./GraphProxyAdmin.sol:34:        require(success);
./GraphProxyAdmin.sol:47:        require(success);
./GraphProxyAdmin.sol:59:        require(success);
./GraphProxy.sol:105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
./GraphProxy.sol:133:        require(success);
./GraphProxy.sol:141:        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
./GraphProxy.sol:142:        require(
                                 _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
                                 "Caller must be the pending implementation"
                             );
./GraphProxy.sol:157:        require(msg.sender != _admin(), "Cannot fallback to proxy target");
./L2GraphToken.sol:36:        require(msg.sender == gateway, "NOT_GATEWAY");
./L2GraphToken.sol:49:        require(_owner != address(0), "Owner must be set");
./L2GraphToken.sol:60:        require(_gw != address(0), "INVALID_GATEWAY");
./L2GraphToken.sol:70:        require(_addr != address(0), "INVALID_L1_ADDRESS");
./GraphTokenUpgradeable.sol:60:        require(isMinter(msg.sender), "Only minter can call");
./GraphTokenUpgradeable.sol:94:        require(_owner == recoveredAddress, "GRT: invalid permit");
./GraphTokenUpgradeable.sol:95:        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
./GraphTokenUpgradeable.sol:106:        require(_account != address(0), "INVALID_MINTER");
./GraphTokenUpgradeable.sol:115:        require(_minters[_account], "NOT_A_MINTER");
./GraphTokenUpgradeable.sol:123:        require(_minters[msg.sender], "NOT_A_MINTER");
./GraphUpgradeable.sol:24:        require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
./GraphUpgradeable.sol:32:        require(msg.sender == _implementation(), "Caller must be the implementation");
./GraphProxyStorage.sol:62:        require(msg.sender == _admin(), "Caller must be admin");
./GraphTokenGateway.sol:19:        require(
                                       msg.sender == controller.getGovernor() || msg.sender == pauseGuardian,
                                       "Only Governor or Guardian can call"
                                   );
./GraphTokenGateway.sol:31:        require(_newPauseGuardian != address(0), "PauseGuardian must be set");
./GraphTokenGateway.sol:40:        require(!_paused, "Paused (contract)");
./L1GraphTokenGateway.sol:74:        require(inbox != address(0), "INBOX_NOT_SET");
./L1GraphTokenGateway.sol:78:        require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");
./L1GraphTokenGateway.sol:82:        require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");
./L1GraphTokenGateway.sol:110:        require(_inbox != address(0), "INVALID_INBOX");
./L1GraphTokenGateway.sol:111:        require(_l1Router != address(0), "INVALID_L1_ROUTER");
./L1GraphTokenGateway.sol:122:        require(_l2GRT != address(0), "INVALID_L2_GRT");
./L1GraphTokenGateway.sol:132:        require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");
./L1GraphTokenGateway.sol:142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
./L1GraphTokenGateway.sol:153:        require(_newWhitelisted != address(0), "INVALID_ADDRESS");
./L1GraphTokenGateway.sol:154:        require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
./L1GraphTokenGateway.sol:165:        require(_notWhitelisted != address(0), "INVALID_ADDRESS");
./L1GraphTokenGateway.sol:166:        require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
./L1GraphTokenGateway.sol:200:        require(_l1Token == address(token), "TOKEN_NOT_GRT");
./L1GraphTokenGateway.sol:201:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
./L1GraphTokenGateway.sol:202:        require(_to != address(0), "INVALID_DESTINATION");
./L1GraphTokenGateway.sol:213:                require(
                                                  extraData.length == 0 || callhookWhitelist[msg.sender] == true,
                                                  "CALL_HOOK_DATA_NOT_ALLOWED"
                                              );
./L1GraphTokenGateway.sol:217:                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
./L1GraphTokenGateway.sol:224:                    require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
./L1GraphTokenGateway.sol:271:        require(_l1Token == address(token), "TOKEN_NOT_GRT");
./L1GraphTokenGateway.sol:275:        require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
./L2GraphTokenGateway.sol:69:        require(
                                         msg.sender == AddressAliasHelper.applyL1ToL2Alias(l1Counterpart),
                                         "ONLY_COUNTERPART_GATEWAY"
                                     );
./L2GraphTokenGateway.sol:98:        require(_l2Router != address(0), "INVALID_L2_ROUTER");
./L2GraphTokenGateway.sol:108:        require(_l1GRT != address(0), "INVALID_L1_GRT");
./L2GraphTokenGateway.sol:118:        require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
./L2GraphTokenGateway.sol:145:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
./L2GraphTokenGateway.sol:146:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
./L2GraphTokenGateway.sol:147:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
./L2GraphTokenGateway.sol:148:        require(_to != address(0), "INVALID_DESTINATION");
./L2GraphTokenGateway.sol:153:        require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
./L2GraphTokenGateway.sol:233:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
./L2GraphTokenGateway.sol:234:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
./Managed.sol:44:        require(!controller.paused(), "Paused");
./Managed.sol:45:        require(!controller.partialPaused(), "Partial-paused");
./Managed.sol:49:        require(!controller.paused(), "Paused");
./Managed.sol:53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
./Managed.sol:57:        require(msg.sender == address(controller), "Caller must be Controller");
./Managed.sol:104:        require(_controller != address(0), "Controller must be set");
```

#### Mitigation
I suggest implementing custom errors to save gas.

&ensp;