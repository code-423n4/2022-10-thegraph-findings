## [NAZ-G1] In `require()`, Use `!= 0` Instead of `> 0` With Uint Values
**Context**: [`L2GraphTokenGateway.sol#L146`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L146), [`L1GraphTokenGateway.sol#L201`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L201), [`L1GraphTokenGateway.sol#L217`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L217)

**Description**:
In a require, when checking a uint, using `!= 0` instead of `> 0` saves 6 gas. This will jump over or avoid an extra `ISZERO` opcode.

**Recommendation**: 
Use `!= 0` instead of `> 0` with uint values but only in `require()` statements.


## [NAZ-G2] Use of `2**256 - 1 && type(uint256).max` When `2**255` Can Be Used
**Context**: [`BridgeEscrow.sol#L29`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L29)

**Description**:
Infinity can also be represented via ``2**255`, it's hex representation is `0x8000000000000000000000000000000000000000000000000000000000000000` while `2**256 - 1` is `0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff`. Then main difference is and where the gas savings come from is, zeros are cheaper than non-zero values in hex representation.

**Recommendation**: 
Use `2**255` instead of `2**256 - 1` to save gas on deployment.


## [NAZ-G3] Setting The Constructor To Payable
**Context**: [`GraphProxyAdmin.sol`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol), [`GraphProxy.sol`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol)

**Description**:
You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of `msg.value == 0` and saves 21 gas on deployment with no security risks.

**Recommendation**: 
Set the constructor to payable.


## [NAZ-G4] Function Ordering via Method ID
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts)

**Description**:
Contracts most called functions could simply save gas by function ordering via Method ID. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because 22 gas are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. One could use [`This tool`](https://emn178.github.io/solidity-optimize-name/) to help find alternative function names with lower Method IDs while keeping the original name intact.

**Recommendation**: 
Find a lower method ID name for the most called functions for example ```mostCalled()``` vs. ```mostCalled_41q()``` is cheaper by 44 gas.
