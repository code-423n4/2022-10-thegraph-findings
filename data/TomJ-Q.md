## Table of Contents
### Low Risk Issues
- Unhandled Return Values of transferFrom
- Require should be used instead of Assert
- Use fixed compiler versions instead of floating version
- Use of block.timestamp

### Non-critical Issues
- Require Statements without Descriptive Revert Strings

&ensp;
## Low Risk Issues
### Unhandled Return Values of transfer and transferFrom

#### Issue
There are function in which ERC20 token tranferFrom is not checking the return values.
Since some of the ERC20 tokens does not revert on failure but return false instead, 
it is important to check the return values or otherwise these tokens is still counted as a correct transfer
even thought it didn't actually perform the transfer.

Similar issue reported in public audit:
https://consensys.net/diligence/audits/2020/09/aave-protocol-v2/#unhandled-return-values-of-transfer-and-transferfrom

#### PoC
```solidity
L1GraphTokenGateway.sol:outboundTransfer():
235:                token.transferFrom(from, escrow, _amount);
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L235
```solidity
L1GraphTokenGateway.sol:finalizeInboundTransfer():
276:        token.transferFrom(escrow, _to, _amount);
```
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L276

#### Mitigation
I recommend to add a require() statement that checks the return values or use OpenZeppelin's SafeERC20 wrapper functions.

&ensp;
### Require should be used instead of Assert

#### Issue
Solidity documents mention that properly functioning code should never reach a failing assert statement
and if this happens there is a bug in the contract which should be fixed.
Reference: https://docs.soliditylang.org/en/v0.8.15/control-structures.html#panic-via-assert-and-error-via-require

#### PoC
Total of 3 instances found.
```
./GraphProxy.sol:47:        assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
./GraphProxy.sol:48:        assert(
                                IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)
                            );
./GraphProxy.sol:51:        assert(
                                PENDING_IMPLEMENTATION_SLOT ==
                                    bytes32(uint256(keccak256("eip1967.proxy.pendingImplementation")) - 1)
                            );
```

#### Mitigation
Replace assert by require.

&ensp;
### Use fixed compiler versions instead of floating version

#### Issue
It is best practice to lock your pragma instead of using floating pragma.
The use of floating pragma has a risk of accidentally get deployed using latest complier
which may have higher risk of undiscovered bugs.
Reference: https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/locking-pragmas/

#### PoC
Total of 23 instances found.
```
./Governed.sol:3:pragma solidity ^0.7.6;
./BridgeEscrow.sol:3:pragma solidity ^0.7.6;
./GraphProxyAdmin.sol:3:pragma solidity ^0.7.6;
./GraphProxy.sol:3:pragma solidity ^0.7.6;
./Pausable.sol:3:pragma solidity ^0.7.6;
./L2GraphToken.sol:3:pragma solidity ^0.7.6;
./GraphTokenUpgradeable.sol:3:pragma solidity ^0.7.6;
./GraphUpgradeable.sol:3:pragma solidity ^0.7.6;
./ICuration.sol:3:pragma solidity ^0.7.6;
./GraphProxyStorage.sol:3:pragma solidity ^0.7.6;
./GraphTokenGateway.sol:3:pragma solidity ^0.7.6;
./IGraphToken.sol:3:pragma solidity ^0.7.6;
./ICallhookReceiver.sol:9:pragma solidity ^0.7.6;
./IStakingData.sol:3:pragma solidity >=0.6.12 <0.8.0;
./L1GraphTokenGateway.sol:3:pragma solidity ^0.7.6;
./L2GraphTokenGateway.sol:3:pragma solidity ^0.7.6;
./Managed.sol:3:pragma solidity ^0.7.6;
./IStaking.sol:3:pragma solidity >=0.6.12 <0.8.0;
./IEpochManager.sol:3:pragma solidity ^0.7.6;
./IGraphCurationToken.sol:3:pragma solidity ^0.7.6;
./IGraphProxy.sol:3:pragma solidity ^0.7.6;
./IRewardsManager.sol:3:pragma solidity ^0.7.6;
./IController.sol:3:pragma solidity >=0.6.12 <0.8.0;
```

#### Mitigation
I suggest to lock your pragma and aviod using floating pragma.
```
// bad
pragma solidity ^0.8.10;

// good
pragma solidity 0.8.10;
```

&ensp;
### Use of block.timestamp

#### Issue
When smart contract utilizes the `block.timestamp` function when performing critical logic
such as time-triggered data or generating random numbers, it may cause vulnerability since
minors can manipulate this block.timestamps freely. 
For random number generation, use a robust algorithm. For functions requiring time-triggered data,
assess whether or not the scale of the time-dependent event can vary by 15 seconds and maintain integrity.
If so, it may be safe to use a block.timestamp for that function. 
Reference: https://halborn.com/what-is-timestamp-dependence/

#### PoC
```solidity
./Pausable.sol:32:            lastPausePartialTime = block.timestamp;
./Pausable.sol:46:            lastPauseTime = block.timestamp;
./GraphTokenUpgradeable.sol:95:        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```

&ensp;
## Non-critical Issues
### Require Statements without Descriptive Revert Strings

#### Issue
It is best practice to include descriptive revert strings for require statement for readability and auditing.

#### PoC
```
./GraphProxyAdmin.sol:34:        require(success);
./GraphProxyAdmin.sol:47:        require(success);
./GraphProxyAdmin.sol:59:        require(success);
./GraphProxy.sol:133:        require(success);
```

#### Mitigation
Add descriptive revert strings to easier understand what the code is trying to do.

&ensp;