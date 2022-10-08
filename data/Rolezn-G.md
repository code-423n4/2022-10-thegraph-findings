## Summary<a name="Summary">

### Gas Optimizations
| |Issue|Instances|
|-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate | 1 |
| [GAS&#x2011;2](#GAS&#x2011;2) | `require()`/`revert()` Strings Longer Than 32 Bytes Cost Extra Gas | 4 |
| [GAS&#x2011;3](#GAS&#x2011;3) | Using Bools For Storage Incurs Overhead | 2 |
| [GAS&#x2011;4](#GAS&#x2011;4) | Using `> 0` Costs More Gas Than `!= 0` When Used On A Uint In A `require()` Statement | 3 |
| [GAS&#x2011;5](#GAS&#x2011;5) | Splitting `require()` Statements That Use `&&` Saves Gas | 1 |
| [GAS&#x2011;6](#GAS&#x2011;6) | `abi.encode()` Is Less Efficient Than `abi.encodepacked()` | 6 |
| [GAS&#x2011;7](#GAS&#x2011;7) | Use assembly to check for `address(0)` | 18 |
| [GAS&#x2011;8](#GAS&#x2011;8) | Public Functions To External | 11 |
| [GAS&#x2011;9](#GAS&#x2011;9) | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 3 |
| [GAS&#x2011;10](#GAS&#x2011;10) | Use of Custom Errors Instead of String | 55 |
| [GAS&#x2011;11](#GAS&#x2011;11) | Optimize names to save gas | 12 |

Total: 116 instances over 11 issues

## Gas Optimizations

### <a href="#Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

#### <ins>Proof Of Concept</ins>


```
mapping(address => bool) private _minters;
mapping(address => uint256) public nonces;

```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L51






### <a href="#Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> `require()`/`revert()` Strings Longer Than 32 Bytes Cost Extra Gas

#### <ins>Proof Of Concept</ins>


```
require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L53

```
require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L105

```
require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L141

```
require(msg.sender != _admin(), "Cannot fallback to proxy target");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L157





### <a href="#Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> Using Bools For Storage Incurs Overhead

Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

#### <ins>Proof Of Concept</ins>


```
    mapping(address => bool) public callhookWhitelist;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L35

```
    mapping(address => bool) private _minters;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L51





### <a href="#Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> Using `> 0` Costs More Gas Than `!= 0` When Used On A Uint In A `require()` Statement

This change saves 6 gas per instance

#### <ins>Proof Of Concept</ins>


```
require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L201

```
require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L216

```
require(_amount > 0, "INVALID_ZERO_AMOUNT");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L144





### <a href="#Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> Splitting `require()` Statements That Use `&&` Saves Gas

See https://github.com/code-423n4/2022-01-xdefi-findings/issues/128 which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

#### <ins>Proof Of Concept</ins>

```
require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L142





### <a href="#Summary">[GAS&#x2011;6]</a><a name="GAS&#x2011;6"> `abi.encode()` Is Less Efficient Than `abi.encodepacked()`

#### <ins>Proof Of Concept</ins>


```
return abi.encode(seqNum);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L249

```
abi.encode(emptyBytes, _data)
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L342

```
return abi.encode(id);
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L174

```
abi.encode(0, _data));
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L275

```
abi.encode(PERMIT_TYPEHASH, _owner, _spender, _value, nonces[_owner], _deadline)
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L88

```
abi.encode(
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L161





### <a href="#Summary">[GAS&#x2011;7]</a><a name="GAS&#x2011;7"> Use assembly to check for `address(0)`

Save 6 gas per instance if using assembly to check for address(0)

e.g.
assembly {
	if iszero(_addr) {
		mstore(0x00, "AddressZero")
		revert(0x00, 0x20)
	}
}

#### <ins>Proof Of Concept</ins>


```
function setPauseGuardian(address _newPauseGuardian) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L30

```
function setArbitrumAddresses(address _inbox, address _l1Router) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L109

```
function setArbitrumAddresses(address _inbox, address _l1Router) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L109

```
function setL2TokenAddress(address _l2GRT) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L121

```
function setL2CounterpartAddress(address _l2Counterpart) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L131

```
function setEscrowAddress(address _escrow) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L141

```
function addToCallhookWhitelist(address _newWhitelisted) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L152

```
function removeFromCallhookWhitelist(address _notWhitelisted) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L164

```
function transferOwnership(address _newGovernor) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Governed.sol#L40

```
function _setController(address _controller) internal {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L103

```
function setL2Router(address _l2Router) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L97

```
function setL1TokenAddress(address _l1GRT) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L107

```
function setL1CounterpartAddress(address _l1Counterpart) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L117

```
function addMinter(address _account) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L105

```
function initialize(address _owner) external onlyImpl {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L48

```
function setGateway(address _gw) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L59

```
function setL1Address(address _addr) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L69

```
function setAdmin(address _newAdmin) external ifAdmin {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L104






### <a href="#Summary">[GAS&#x2011;8]</a><a name="GAS&#x2011;8"> Public Functions To External

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

#### <ins>Proof Of Concept</ins>


```
function getOutboundCalldata(
        address _l1Token,
        address _from,
        address _to,
        uint256 _amount,
        bytes memory _data
    ) public pure returns (bytes memory) {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L326

```
function outboundTransfer(
        address _l1Token,
        address _to,
        uint256 _amount,
        uint256,         uint256,         bytes calldata _data
    ) public payable override notPaused nonReentrant returns (bytes memory) {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L137

```
function calculateL2TokenAddress(address l1ERC20) public view override returns (address) {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L203

```
function getOutboundCalldata(
        address _token,
        address _from,
        address _to,
        uint256 _amount,
        bytes memory _data
    ) public pure returns (bytes memory) {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L261

```
function isMinter(address _account) public view returns (bool) {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L141

```
function getProxyImplementation(IGraphProxy _proxy) public view returns (address) {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L30

```
function getProxyPendingImplementation(IGraphProxy _proxy) public view returns (address) {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L43

```
function getProxyAdmin(IGraphProxy _proxy) public view returns (address) {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L55

```
function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L68

```
function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L77

```
function acceptProxy(GraphUpgradeable _implementation, IGraphProxy _proxy) public onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol#L86






### <a href="#Summary">[GAS&#x2011;9]</a><a name="GAS&#x2011;9"> Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

#### <ins>Proof Of Concept</ins>


```
uint32 cooldownBlocks;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStakingData.sol#L37

```
uint32 indexingRewardCut;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStakingData.sol#L38

```
uint32 queryFeeCut;
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/staking/IStakingData.sol#L39






### <a href="#Summary">[GAS&#x2011;10]</a><a name="GAS&#x2011;10"> Use of Custom Errors Instead of String

To save some gas the use of custom errors leads to cheaper deploy time cost and run time cost. The run time cost is only relevant when the revert condition is met.

#### <ins>Proof Of Concept</ins>


```
File: \2022-10-thegraph\contracts\gateway\GraphTokenGateway.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L40

```
File: \2022-10-thegraph\contracts\gateway\L1GraphTokenGateway.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L275

```
File: \2022-10-thegraph\contracts\governance\Governed.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Governed.sol#L41

```
File: \2022-10-thegraph\contracts\governance\Managed.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L104

```
File: \2022-10-thegraph\contracts\l2\gateway\L2GraphTokenGateway.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L147

```
File: \2022-10-thegraph\contracts\l2\token\GraphTokenUpgradeable.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L123

```
File: \2022-10-thegraph\contracts\l2\token\L2GraphToken.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L70

```
File: \2022-10-thegraph\contracts\upgrades\GraphProxy.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L157

```
File: \2022-10-thegraph\contracts\upgrades\GraphProxyStorage.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol#L62

```
File: \2022-10-thegraph\contracts\upgrades\GraphUpgradeable.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphUpgradeable.sol#L32



#### <ins>Recommended Mitigation Steps</ins>

Use Custom Errors instead of strings.



### <a href="#Summary">[GAS&#x2011;11]</a><a name="GAS&#x2011;11"> Optimize names to save gas

`public`/`external` function names and `public` member variable names can be optimized to save gas. See [this](https://github.com/enzosv/solidity-optimize-name) link for an example of how it works. Method IDs that have two leading zero bytes can save **128 gas** each during deployment, and renaming functions to have lower method IDs will save **22 gas** per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92)

#### <ins>Proof Of Concept</ins>

```
File: \2022-10-thegraph\contracts\gateway\GraphTokenGateway.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol

```
File: \2022-10-thegraph\contracts\gateway\L1GraphTokenGateway.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol

```
File: \2022-10-thegraph\contracts\governance\Governed.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Governed.sol

```
File: \2022-10-thegraph\contracts\governance\Managed.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol

```
File: \2022-10-thegraph\contracts\governance\Pausable.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Pausable.sol

```
File: \2022-10-thegraph\contracts\l2\gateway\L2GraphTokenGateway.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol

```
File: \2022-10-thegraph\contracts\l2\token\GraphTokenUpgradeable.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol

```
File: \2022-10-thegraph\contracts\l2\token\L2GraphToken.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol

```
File: \2022-10-thegraph\contracts\upgrades\GraphProxy.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol

```
File: \2022-10-thegraph\contracts\upgrades\GraphProxyAdmin.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyAdmin.sol

```
File: \2022-10-thegraph\contracts\upgrades\GraphProxyStorage.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol

```
File: \2022-10-thegraph\contracts\upgrades\GraphUpgradeable.sol
```

https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphUpgradeable.sol






