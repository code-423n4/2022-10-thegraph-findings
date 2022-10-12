## [NAZ-L1] Missing Zero-address Validation
**Severity**: Low
**Context**: [`Governed.sol#L32`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L32), [`GraphTokenupgradeable.sol#L150`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L150), [`GraphProxyStorage.sol#L80`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L80), [`GraphProxyStorage.sol#L115`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L115)

**Description**:
Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

**Recommendation**:
Consider adding explicit zero-address validation on input parameters of address type.


## [NAZ-L2] `DOMAIN_TYPEHASH` Can Change 
**Severity**: Low
**Context**: [`GraphTokenUpgradeable.sol#L34`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L34)

**Description**:
The variable `DOMAIN_TYPEHASH` is hard coded and will not change after being initialized. However, if a hard fork happens after the contract deployment, the domain would become invalid on one of the forked chains due to the `block.chainid` has changed. Also, you don't need an assmebly to retrieve `chainid`, you can get it from a built in variable `block.chainid`.

**Recommendation**:
Consider the solution from [Sushi Trident](https://github.com/sushiswap/trident/blob/concentrated/contracts/pool/concentrated/TridentNFT.sol#L47-L62).


## [NAZ-L3] Max/Infinite Approvals are Dangerous
**Severity**: Low
**Context**: [`BridgeEscrow.sol#L29`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L29)

**Description**:
Giving max/infinite approvals to contracts are dangerous because if those contracts are exploited then they can remove all the funds from the approving addresses.

**Recommendation**:
Consider checking allowance and approve as much as required.


## [NAZ-L4] Missing Events In Initialize Functions
**Severity**: Low
**Context**: [`Governed.sol#L31`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L31), [`GraphTokenUpgradeable.sol#L150`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L150), [`L2GraphTokenGateway.sol#L89`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L89), [`L1GraphTokenGateway.sol#L99`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L99)

**Description**:
None of the initialize functions emit emit init-specific events. They all however have the initializer modifier (from Initializable) so that they can be called only once. Off-chain monitoring of calls to these critical functions is not possible.

**Recommendation**:
It is recommended to emit events in your initialization functions.


## [NAZ-N1] Function && Variable Naming Convention
**Severity** Informational
**Context**: [`GraphUpgradeable.sol#L17`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L17), [`L1GraphTokenGateway.sol#L290`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L290)

**Description**:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `_underscore`.

**Recommendation**:
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html). 


## [NAZ-N2] Code Structure Deviates From Best-Practice
**Severity**: Informational
**Context**: [`GraphUpgradeable.sol#L50`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L50), [`Governed.sol#L40`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.solL40), [`L1GraphTokenGateway.sol#L326`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L326)

**Description**:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier.  Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Functions should then further be ordered with view functions coming after the non-view labeled ones.

**Recommendation**:
Consider adopting recommended best-practice for code structure and layout.


## [NAZ-N3] Unclear Revert Messages
**Severity** Informational
**Context**: [`GraphProxyAdmin.sol#L34`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L34), [`GraphProxyAdmin.sol#L47`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L47), [`GraphProxyAdmin.sol#L59`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L59), [`GraphProxy.sol#L133`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L133)

**Description**:
Some revert messages are unclear which can lead to confusion. Unclear revert messages may cause misunderstandings on reverted transactions.

**Recommendation**: 
Consider making revert messages more clear.


## [NAZ-N4] Spelling Errors
**Severity**: Informational
**Context**: [`L1GraphTokenGateway.sol#L260 (Recepient => Recipient)`](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L260)

**Description**:
Spelling errors in comments can cause confusion to both users and developers.

**Recommendation**:
Consider checking all misspellings to ensure they are corrected.


## [NAZ-N5] Missing or Incomplete NatSpec
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts)

**Description**:
Some functions are missing @notice/@dev NatSpec comments for the function, @param for all/some of their parameters and @return for return values. Given that NatSpec is an important part of code documentation, this affects code comprehension, auditability and usability.

**Recommendation**:
Consider adding in full NatSpec comments for all functions to have complete code documentation for future use.


## [NAZ-N6] Floating Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts)

**Description**:
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

**Recommendation**: 
Consider locking the pragma version.