# [01] Avoid floating the pragma and consider updating the solidity version where possible

All the contracts in scope are using `v0.7.6`.

Locking the pragma helps to ensure that contracts do not accidentally get deployed using an outdated compiler version. Note that pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or a package.

Furthermore, using the latest version of solidity ensures the compiler has the latest security fixes and also brings improved features. E.g.

Version of at least 0.8.0 allows overflow protection without SafeMath.

Version of at least 0.8.2 allows compiler automatic inlining.

Version of at least 0.8.3 allows better struct packing and cheaper multiple storage reads.

Version of at least 0.8.4 allows custom errors, which are cheaper at deployment than revert()/require() strings.

# [02] Check the return of `transfer/transferFrom` and `approve`

The `ERC20` interface has a boolean signature to indicate the successful return of `transfer/transferFrom` and `approve`.
Checking the returns helps to prevent unexpected behavior on external calls.
Consider checking the return of the following ERC20 operations for the GRT token:

```
token.transferFrom(escrow, _to, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L276

```
token.transferFrom(from, escrow, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L235

```
graphToken().approve(_spender, type(uint256).max);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L29

# [03] Validate the return of `ECDSA.recover` agaist adress zero

When calling `ECDSA.recover`, if the signature does not match, the address zero will be returned. If `_owner` is also address zero, the require statement on `GraphTokenUpgradeable` [line 94](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L94) will pass.

Consider checking the return of `ECDSA.recover`. E.g.

```
diff --git a/GraphTokenUpgradeable.sol.orig b/GraphTokenUpgradeable.sol
index b8f0314..98fe26b 100644
--- a/GraphTokenUpgradeable.sol.orig
+++ b/GraphTokenUpgradeable.sol
@@ -91,7 +91,7 @@ contract GraphTokenUpgradeable is GraphUpgradeable, Governed, ERC20BurnableUpgra
         );

         address recoveredAddress = ECDSA.recover(digest, _v, _r, _s);
-        require(_owner == recoveredAddress, "GRT: invalid permit");
+        require(recoveredAddress != address(0) && _owner == recoveredAddress, "GRT: invalid permit");
         require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
```

# [04] Missing address(0) validation check

`Governed.initialize()` should contain an address(0) validation. It's called by `L2GraphTokenGateway.initialize()` and `GraphTokenUpgradeable.initialize()`.

If this variable gets configured with address zero, failure to immediately reset the value can result in unexpected behavior for the project.

```
function _initialize(address _initGovernor) internal {
    governor = _initGovernor;
}
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L31-L33

```
function initialize(address _controller) external onlyImpl {
    Managed._initialize(_controller);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L87-L88

```
function _initialize(address _owner, uint256 _initialSupply) internal {
    __ERC20_init("Graph Token", "GRT");
    Governed._initialize(_owner);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L150-L152

# [05] Inside `finalizeInboundTransfer()`, the `_from` argument is being emitted inside the event, but the transfer is sent from the `escrow` address

The `_from` argument inside `finalizeInboundTransfer` is not used (except as an event argument). The transfer is implemented in behalf of the `escrow`.

```
function finalizeInboundTransfer(
    address _l1Token,
    address _from,
    address _to,
    uint256 _amount,
    bytes calldata // _data, contains exitNum, unused by this contract
) external payable override notPaused onlyL2Counterpart {
    IGraphToken token = graphToken();
    require(_l1Token == address(token), "TOKEN_NOT_GRT");

    uint256 escrowBalance = token.balanceOf(escrow);
    // If the bridge doesn't have enough tokens, something's very wrong!
    require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
    token.transferFrom(escrow, _to, _amount);

    emit WithdrawalFinalized(_l1Token, _from, _to, 0, _amount);
}
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L263-L279

## Recommendation

Remove the `_from` argument, and include `escrow` in the event. 

It's important to note that if the `escrow` is set incorrectly in `L1GraphTokenGateway.setEscrowAddress()`, it's possible to send funds from another contract to the `_to` address, resulting in a potential loss of funds on the hypothetical incorrect address setted as `escrow`.

# [06] Usage of outdated OpenZeppelin libraries

The project is using v^3.4.1 for `openzeppelin/contracts` and v3.4.2 for `@openzeppelin/contracts-upgradeable`. The latest stable version for these libraries is v4.7.3. Consider updating the version for OpenZeppelin to ensure the security of the library contracts are up-to-date.

# [07] Critical changes should use two-step procedure

Lack of two-step procedure for critical operations leaves them error-prone.

Consider adding a two-steps pattern on critical changes to avoid mistakenly transferring ownership of roles or critial functionalities to the wrong address.

```
function setEscrowAddress(address _escrow) external onlyGovernor {
    require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
    escrow = _escrow;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L141-L143

```
function setGateway(address _gw) external onlyGovernor {
    require(_gw != address(0), "INVALID_GATEWAY");
    gateway = _gw;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L59-L61

# [08] Event emitted after external call

If `finalizeInboundTransfer()` re-enters, the events will be emitted in the wrong order , therefore impacting offchain monitoring.

Consider emitting the event before the external call.

```
token.transferFrom(escrow, _to, _amount);
emit WithdrawalFinalized(_l1Token, _from, _to, 0, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L276-L278

# [09] `_pendingImplementation` is being showdowed

The variable named `_pendingImplementation` is shadowing the function that it extracts its value from `GraphProxyStorage._pendingImplementation`. This won't cause a collision, but it will warn on linters/slither/extensions. Consider renaming the variable `_pendingImplementation` to ensure the project has the least amount of warnings from linter outputs.

```
address _pendingImplementation = _pendingImplementation();
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L140

# [10] `GraphProxy` should inherit from `IGraxyProxy`

```
contract GraphProxy is GraphProxyStorage {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L16

# [11] Missing NATSPEC/documentation

Consider adding NATSPEC on all functions to improve documentation

```
function _notPartialPaused() internal view {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L43

# [12] Replace memory with calldata for read-only referential function arguments

```
function parseOutboundData(bytes memory _data)
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L290

```
function _syncContract(string memory _name) internal {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L173

# [13] Order of functions

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) recommends the following order for functions:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private

The instances bellow shows private above public and external bellow public. Consider adjusting the order of funtions as recommended by the solidify style guide.

```
function parseOutboundData(bytes memory _data)
    private
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L290-L291

```
function getOutboundCalldata(
    address _l1Token,
    address _from,
    address _to,
    uint256 _amount,
    bytes memory _data
) public pure returns (bytes memory) {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L326-L332

```
function calculateL2TokenAddress(address _l1ERC20) external view override returns (address) {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L352

# [14] Disallow updating the escrow with the current escrow

Currently, it possible to generate an event `EscrowAddressSet` even if the `_escrow` argument is the same as the current `escrow` state variable. Generating an event in this case might cause confusion since it will be the same address.

```
function setEscrowAddress(address _escrow) external onlyGovernor {
    require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
    escrow = _escrow;
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L141-L142

## Recommendation

Add a check ensuring that the `_escrow` argument does not equal the existing `escrow`.

# [15] `GraphProxyAdmin.acceptProxy()` can be external instead of public

```
function acceptProxy(GraphUpgradeable _implementation, IGraphProxy _proxy) public onlyGovernor {
```   

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L86

# [16] Replace assert with require or custom error

Assert should be avoided in production code. As described on the solidity [documentation](https://docs.soliditylang.org/en/v0.8.15/control-structures.html#panic-via-assert-and-error-via-require). "The assert function creates an error of type Panic(uint256). … Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix."
Even if the code is expected to be unreacheable, consider using a require statement or a custom error instead of assert.

There are 3 instances of this issue.

```
File: contracts/upgrades/GraphProxy.sol
47: assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
48: assert(
51: assert(
```

# [17] Variable `expectedEth` can be removed

The variable `expectedEth` is only used once. Hence, `maxSubmissionCost.add(_maxGas.mul(_gasPriceBid))` can be used directly inside the require statement.

```
uint256 expectedEth = maxSubmissionCost.add(_maxGas.mul(_gasPriceBid));
require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L223-L224

Can be replace to:

```
require(msg.value >= maxSubmissionCost.add(_maxGas.mul(_gasPriceBid)), "WRONG_ETH_VALUE");
```
