## Summary

### Low Risk Issues
| |Issue|Instances|
|-|:-|:-:|
| [L&#x2011;01] | Upgradeable contract not initialized | 1 |
| [L&#x2011;02] | `initialize()` function does not use the `initializer` modifier | 1 |
| [L&#x2011;03] | `require()` should be used instead of `assert()` | 3 |
| [L&#x2011;04] | Missing checks for `address(0x0)` when assigning values to `address` state variables | 1 |
| [L&#x2011;05] | `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()` | 1 |
| [L&#x2011;06] | Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions | 4 |

Total: 11 instances over 6 issues

### Non-critical Issues
| |Issue|Instances|
|-|:-|:-:|
| [N&#x2011;01] | Missing `initializer` modifier on constructor | 1 |
| [N&#x2011;02] | Missing `initializer` modifier | 1 |
| [N&#x2011;03] | The `nonReentrant` `modifier` should occur before all other modifiers | 2 |
| [N&#x2011;04] | `require()`/`revert()` statements should have descriptive reason strings | 4 |
| [N&#x2011;05] | `public` functions not called by the contract should be declared `external` instead | 6 |
| [N&#x2011;06] | Non-assembly method available | 1 |
| [N&#x2011;07] | `constant`s should be defined rather than using magic numbers | 2 |
| [N&#x2011;08] | Cast is more restrictive than the type of the variable being assigned | 1 |
| [N&#x2011;09] | Use a more recent version of solidity | 3 |
| [N&#x2011;10] | Use a more recent version of solidity | 2 |
| [N&#x2011;11] | Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant` | 4 |
| [N&#x2011;12] | Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables | 1 |
| [N&#x2011;13] | Non-library/interface files should use fixed compiler versions, not floating ones | 12 |
| [N&#x2011;14] | File is missing NatSpec | 6 |
| [N&#x2011;15] | NatSpec is incomplete | 6 |
| [N&#x2011;16] | Event is missing `indexed` fields | 18 |
| [N&#x2011;17] | Duplicated `require()`/`revert()` checks should be refactored to a modifier or function | 5 |

Total: 74 instances over 17 issues


## Low Risk Issues

### [L&#x2011;01]  Upgradeable contract not initialized
Upgradeable contracts are initialized via an initializer function rather than by a constructor. Leaving such a contract uninitialized may lead to it being taken over by a malicious user

*There is 1 instances of this issue:*
```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

/// @audit missing __ERC20Burnable_init()
28:   contract GraphTokenUpgradeable is GraphUpgradeable, Governed, ERC20BurnableUpgradeable {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/GraphTokenUpgradeable.sol#L28

### [L&#x2011;02]  `initialize()` function does not use the `initializer` modifier
Without the modifier, the function may be called multiple times, overwriting prior initializations

*There is 1 instance of this issue:*
```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

87:       function initialize(address _controller) external onlyImpl {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/gateway/L2GraphTokenGateway.sol#L87

### [L&#x2011;03]  `require()` should be used instead of `assert()`
Prior to solidity version 0.8.0, hitting an assert consumes the **remainder of the transaction's available gas** rather than returning it, as `require()`/`revert()` do. `assert()` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that "The assert function creates an error of type Panic(uint256). ... Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix".

*There are 3 instances of this issue:*
```solidity
File: contracts/upgrades/GraphProxy.sol

47:           assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));

48            assert(
49                IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)
50:           );

51            assert(
52                PENDING_IMPLEMENTATION_SLOT ==
53                    bytes32(uint256(keccak256("eip1967.proxy.pendingImplementation")) - 1)
54:           );

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphProxy.sol#L47

### [L&#x2011;04]  Missing checks for `address(0x0)` when assigning values to `address` state variables

*There is 1 instance of this issue:*
```solidity
File: contracts/governance/Governed.sol

32:           governor = _initGovernor;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Governed.sol#L32

### [L&#x2011;05]  `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`
Use `abi.encode()` instead which will pad items to 32 bytes, which will [prevent hash collisions](https://docs.soliditylang.org/en/v0.8.13/abi-spec.html#non-standard-packed-mode) (e.g. `abi.encodePacked(0x123,0x456)` => `0x123456` => `abi.encodePacked(0x1,0x23456)`, but `abi.encode(0x123,0x456)` => `0x0...1230...456`). "Unless there is a compelling reason, `abi.encode` should be preferred". If there is only one argument to `abi.encodePacked()` it can often be cast to `bytes()` or `bytes32()` [instead](https://ethereum.stackexchange.com/questions/30912/how-to-compare-strings-in-solidity#answer-82739).

If all arguments are strings and or bytes, `bytes.concat()` should be used instead

*There is 1 instance of this issue:*
```solidity
File: contracts/governance/Managed.sol

174:          bytes32 nameHash = keccak256(abi.encodePacked(_name));

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Managed.sol#L174

### [L&#x2011;06]  Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions
See [this](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps) link for a description of this storage variable. While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

*There are 4 instances of this issue:*
```solidity
File: contracts/gateway/BridgeEscrow.sol

15:   contract BridgeEscrow is GraphUpgradeable, Managed {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/gateway/BridgeEscrow.sol#L15

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

23:   contract L2GraphTokenGateway is GraphTokenGateway, L2ArbitrumMessenger, ReentrancyGuardUpgradeable {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/gateway/L2GraphTokenGateway.sol#L23

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

28:   contract GraphTokenUpgradeable is GraphUpgradeable, Governed, ERC20BurnableUpgradeable {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/GraphTokenUpgradeable.sol#L28

```solidity
File: contracts/l2/token/L2GraphToken.sol

15:   contract L2GraphToken is GraphTokenUpgradeable, IArbToken {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/L2GraphToken.sol#L15

## Non-critical Issues

### [N&#x2011;01]  Missing `initializer` modifier on constructor
OpenZeppelin [recommends](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5) that the `initializer` modifier be applied to constructors in order to avoid potential griefs, [social engineering](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/4), or exploits. Ensure that the modifier is applied to the implementation contract. If the default constructor is currently being used, it should be changed to be an explicit one with the modifier applied.

*There is 1 instance of this issue:*
```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

23:   contract L2GraphTokenGateway is GraphTokenGateway, L2ArbitrumMessenger, ReentrancyGuardUpgradeable {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/gateway/L2GraphTokenGateway.sol#L23

### [N&#x2011;02]  Missing `initializer` modifier
The contract extends `Initializable`/`ReentrancyGuardUpgradeable` but does not use the `initializer` modifier anywhere

*There is 1 instance of this issue:*
```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

23:   contract L2GraphTokenGateway is GraphTokenGateway, L2ArbitrumMessenger, ReentrancyGuardUpgradeable {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/gateway/L2GraphTokenGateway.sol#L23

### [N&#x2011;03]  The `nonReentrant` `modifier` should occur before all other modifiers
This is a best-practice to protect against reentrancy in other modifiers

*There are 2 instances of this issue:*
```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

144:      ) public payable override notPaused nonReentrant returns (bytes memory) {

232:      ) external payable override notPaused onlyL1Counterpart nonReentrant {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/gateway/L2GraphTokenGateway.sol#L144

### [N&#x2011;04]  `require()`/`revert()` statements should have descriptive reason strings

*There are 4 instances of this issue:*
```solidity
File: contracts/upgrades/GraphProxyAdmin.sol

34:           require(success);

47:           require(success);

59:           require(success);

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphProxyAdmin.sol#L34

```solidity
File: contracts/upgrades/GraphProxy.sol

133:          require(success);

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphProxy.sol#L133

### [N&#x2011;05]  `public` functions not called by the contract should be declared `external` instead
Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents' functions and change the visibility from `external` to `public`.

*There are 6 instances of this issue:*
```solidity
File: contracts/upgrades/GraphProxyAdmin.sol

30:       function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {

43:       function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {

55:       function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {

68:       function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {

77:       function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {

86:       function acceptProxy(GraphUpgradeable _implementation, IGraphProxy _proxy) public onlyGovernor {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphProxyAdmin.sol#L30

### [N&#x2011;06]  Non-assembly method available 
`assembly{ id := chainid() }` => `uint256 id = block.chainid`, `assembly { size := extcodesize() }` => `uint256 size = address().code.length`
There are some automated tools that will flag a project as having higher complexity if there is inline-assembly, so it's best to avoid using it where it's not necessary

*There is 1 instance of this issue:*
```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

199:              id := chainid()

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/GraphTokenUpgradeable.sol#L199

### [N&#x2011;07]  `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*There are 2 instances of this issue:*
```solidity
File: contracts/upgrades/GraphProxy.sol

/// @audit 0x40
161:              let ptr := mload(0x40)

/// @audit 0xffffffffffffffffffffffffffffffffffffffff
164:              let impl := and(sload(IMPLEMENTATION_SLOT), 0xffffffffffffffffffffffffffffffffffffffff)

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphProxy.sol#L161

### [N&#x2011;08]  Cast is more restrictive than the type of the variable being assigned
If `address foo` is being used in an expression such as `IERC20 token = FooToken(foo)`, then the more specific cast to `FooToken` is a waste because the only thing the compiler will check for is that `FooToken` extends `IERC20` - it won't check any of the function signatures. Therefore, it makes more sense to do `IERC20 token = IERC20(token)` or better yet `FooToken token = FooToken(foo)`. The former may allow the file in which it's used to remove the import for `FooToken`

*There is 1 instance of this issue:*
```solidity
File: contracts/governance/Managed.sol

/// @audit IController vs address
105:          controller = IController(_controller);

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Managed.sol#L105

### [N&#x2011;09]  Use a more recent version of solidity
Use a solidity version of at least 0.8.13 to get the ability to use `using for` with a list of free functions

*There are 3 instances of this issue:*
```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/gateway/L1GraphTokenGateway.sol#L3

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/gateway/L2GraphTokenGateway.sol#L3

```solidity
File: contracts/l2/token/L2GraphToken.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/L2GraphToken.sol#L3

### [N&#x2011;10]  Use a more recent version of solidity
Use a solidity version of at least 0.8.4 to get `bytes.concat()` instead of `abi.encodePacked(<bytes>,<bytes>)`
Use a solidity version of at least 0.8.12 to get `string.concat()` instead of `abi.encodePacked(<str>,<str>)`

*There are 2 instances of this issue:*
```solidity
File: contracts/governance/Managed.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Managed.sol#L3

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/GraphTokenUpgradeable.sol#L3

### [N&#x2011;11]  Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`
While it doesn't save any gas because the compiler knows that developers often make this mistake, it's still best to use the right tool for the task at hand. There is a difference between `constant` variables and `immutable` variables, and they should each be used in their appropriate contexts. `constants` should be used for literal values written into the code, and `immutable` variables should be used for expressions, or values calculated in, or passed into the constructor.

*There are 4 instances of this issue:*
```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

34        bytes32 private constant DOMAIN_TYPE_HASH =
35            keccak256(
36                "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract,bytes32 salt)"
37:           );

38:       bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");

39:       bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");

42        bytes32 private constant PERMIT_TYPEHASH =
43            keccak256(
44                "Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)"
45:           );

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/GraphTokenUpgradeable.sol#L34-L37

### [N&#x2011;12]  Variable names that consist of all capital letters should be reserved for `constant`/`immutable` variables
If the variable needs to be different based on which class it comes from, a `view`/`pure` _function_ should be used instead (e.g. like [this](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/76eee35971c2541585e05cbf258510dda7b2fbc6/contracts/token/ERC20/extensions/draft-IERC20Permit.sol#L59)).

*There is 1 instance of this issue:*
```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

50:       bytes32 private DOMAIN_SEPARATOR;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/GraphTokenUpgradeable.sol#L50

### [N&#x2011;13]  Non-library/interface files should use fixed compiler versions, not floating ones

*There are 12 instances of this issue:*
```solidity
File: contracts/gateway/BridgeEscrow.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/gateway/BridgeEscrow.sol#L3

```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/gateway/L1GraphTokenGateway.sol#L3

```solidity
File: contracts/governance/Governed.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Governed.sol#L3

```solidity
File: contracts/governance/Managed.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Managed.sol#L3

```solidity
File: contracts/governance/Pausable.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Pausable.sol#L3

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/gateway/L2GraphTokenGateway.sol#L3

```solidity
File: contracts/l2/token/GraphTokenUpgradeable.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/GraphTokenUpgradeable.sol#L3

```solidity
File: contracts/l2/token/L2GraphToken.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/L2GraphToken.sol#L3

```solidity
File: contracts/upgrades/GraphProxyAdmin.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphProxyAdmin.sol#L3

```solidity
File: contracts/upgrades/GraphProxy.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphProxy.sol#L3

```solidity
File: contracts/upgrades/GraphProxyStorage.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphProxyStorage.sol#L3

```solidity
File: contracts/upgrades/GraphUpgradeable.sol

3:    pragma solidity ^0.7.6;

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphUpgradeable.sol#L3

### [N&#x2011;14]  File is missing NatSpec

*There are 6 instances of this issue:*
```solidity
File: contracts/curation/ICuration.sol


```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/curation/ICuration.sol

```solidity
File: contracts/curation/IGraphCurationToken.sol


```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/curation/IGraphCurationToken.sol

```solidity
File: contracts/epochs/IEpochManager.sol


```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/epochs/IEpochManager.sol

```solidity
File: contracts/governance/IController.sol


```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/IController.sol

```solidity
File: contracts/token/IGraphToken.sol


```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/token/IGraphToken.sol

```solidity
File: contracts/upgrades/IGraphProxy.sol


```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/IGraphProxy.sol

### [N&#x2011;15]  NatSpec is incomplete

*There are 6 instances of this issue:*
```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

/// @audit Missing: '@param bytes'
252       /**
253        * @notice Receives withdrawn tokens from L2
254        * The equivalent tokens are released from escrow and sent to the destination.
255        * @dev can only accept transactions coming from the L2 GRT Gateway.
256        * The last parameter is unused but kept for compatibility with Arbitrum gateways,
257        * and the encoded exitNum is assumed to be 0.
258        * @param _l1Token L1 Address of the GRT contract (needed for compatibility with Arbitrum Gateway Router)
259        * @param _from Address of the sender
260        * @param _to Recepient address on L1
261        * @param _amount Amount of tokens transferred
262        */
263       function finalizeInboundTransfer(
264           address _l1Token,
265           address _from,
266           address _to,
267           uint256 _amount,
268           bytes calldata // _data, contains exitNum, unused by this contract
269:      ) external payable override notPaused onlyL2Counterpart {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/gateway/L1GraphTokenGateway.sol#L252-L269

```solidity
File: contracts/governance/Managed.sol

/// @audit Missing: '@param _nameHash'
157       /**
158        * @dev Resolve a contract address from the cache or the Controller if not found.
159        * @return Address of the contract
160        */
161:      function _resolveContract(bytes32 _nameHash) internal view returns (address) {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Managed.sol#L157-L161

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

/// @audit Missing: '@param uint256'
123       /**
124        * @notice Burns L2 tokens and initiates a transfer to L1.
125        * The tokens will be available on L1 only after the wait period (7 days) is over,
126        * and will require an Outbox.executeTransaction to finalize.
127        * Note that the caller must previously allow the gateway to spend the specified amount of GRT.
128        * @dev no additional callhook data is allowed. The two unused params are needed
129        * for compatibility with Arbitrum's gateway router.
130        * The function is payable for ITokenGateway compatibility, but msg.value must be zero.
131        * @param _l1Token L1 Address of GRT (needed for compatibility with Arbitrum Gateway Router)
132        * @param _to Recipient address on L1
133        * @param _amount Amount of tokens to burn
134        * @param _data Contains sender and additional data (always empty) to send to L1
135        * @return ID of the withdraw transaction
136        */
137       function outboundTransfer(
138           address _l1Token,
139           address _to,
140           uint256 _amount,
141           uint256, // unused on L2
142           uint256, // unused on L2
143           bytes calldata _data
144:      ) public payable override notPaused nonReentrant returns (bytes memory) {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/gateway/L2GraphTokenGateway.sol#L123-L144

```solidity
File: contracts/upgrades/GraphProxyAdmin.sol

/// @audit Missing: '@param _proxy'
25        /**
26         * @dev Returns the current implementation of a proxy.
27         * This is needed because only the proxy admin can query it.
28         * @return The address of the current implementation of the proxy.
29         */
30:       function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {

/// @audit Missing: '@param _proxy'
38        /**
39         * @dev Returns the pending implementation of a proxy.
40         * This is needed because only the proxy admin can query it.
41         * @return The address of the pending implementation of the proxy.
42         */
43:       function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {

/// @audit Missing: '@param _proxy'
51        /**
52         * @dev Returns the admin of a proxy. Only the admin can query it.
53         * @return The address of the current admin of the proxy.
54         */
55:       function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphProxyAdmin.sol#L25-L30

### [N&#x2011;16]  Event is missing `indexed` fields
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

*There are 18 instances of this issue:*
```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

56:       event ArbitrumAddressesSet(address inbox, address l1Router);

58:       event L2TokenAddressSet(address l2GRT);

60:       event L2CounterpartAddressSet(address l2Counterpart);

62:       event EscrowAddressSet(address escrow);

64:       event AddedToCallhookWhitelist(address newWhitelisted);

66:       event RemovedFromCallhookWhitelist(address notWhitelisted);

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/gateway/L1GraphTokenGateway.sol#L56

```solidity
File: contracts/governance/Managed.sol

33:       event ParameterUpdated(string param);

34:       event SetController(address controller);

39:       event ContractSynced(bytes32 indexed nameHash, address contractAddress);

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Managed.sol#L33

```solidity
File: contracts/governance/Pausable.sol

19:       event PartialPauseChanged(bool isPaused);

20:       event PauseChanged(bool isPaused);

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Pausable.sol#L19

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

58:       event L2RouterSet(address l2Router);

60:       event L1TokenAddressSet(address l1GRT);

62:       event L1CounterpartAddressSet(address l1Counterpart);

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/gateway/L2GraphTokenGateway.sol#L58

```solidity
File: contracts/l2/token/L2GraphToken.sol

24:       event BridgeMinted(address indexed account, uint256 amount);

26:       event BridgeBurned(address indexed account, uint256 amount);

28:       event GatewaySet(address gateway);

30:       event L1AddressSet(address l1Address);

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/token/L2GraphToken.sol#L24

### [N&#x2011;17]  Duplicated `require()`/`revert()` checks should be refactored to a modifier or function
The compiler will inline the function, which will avoid `JUMP` instructions usually associated with functions

*There are 5 instances of this issue:*
```solidity
File: contracts/gateway/L1GraphTokenGateway.sol

271:          require(_l1Token == address(token), "TOKEN_NOT_GRT");

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/gateway/L1GraphTokenGateway.sol#L271

```solidity
File: contracts/governance/Managed.sol

49:           require(!controller.paused(), "Paused");

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/governance/Managed.sol#L49

```solidity
File: contracts/l2/gateway/L2GraphTokenGateway.sol

233:          require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

234:          require(msg.value == 0, "INVALID_NONZERO_VALUE");

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/l2/gateway/L2GraphTokenGateway.sol#L233

```solidity
File: contracts/upgrades/GraphProxyAdmin.sol

47:           require(success);

```
https://github.com/code-423n4/2022-10-thegraph/blob/48e7c7cf641847e07ba07e01176cb17ba8ad6432/contracts/upgrades/GraphProxyAdmin.sol#L47



