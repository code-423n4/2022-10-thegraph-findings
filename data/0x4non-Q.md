# Low 

## Missing Initializer pattern
With current pattern the implementation could call multiple times the `initialize` method with different data.

On 
- [contracts/gateway/BridgeEscrow.sol/#L20](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol/#L20)
- [contracts/gateway/L1GraphTokenGateway.sol/#L99](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol/#L99)
- [contracts/l2/gateway/L2GraphTokenGateway.sol/#L87](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol/#L87)
- [contracts/l2/token/L2GraphToken.sol/#L48](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol/#L48)


## Missing gap space for upgradeability

Missing a __gap[50] storage variable to allow for new storage variables in later versions.
This is empty reserved space in storage that is put in place in Upgradeable contracts. It allows us to freely add new state variables in the future without compromising the storage compatibility with existing deployments.

Lines
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol/#L25
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol/#L38

Looking at the OpenZeppelin [UUPSUpgradeable.sol:L107](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/master/contracts/proxy/utils/UUPSUpgradeable.sol#L107) Implementation we can see the `__gap` variable is present there at Line 107

## Missing address(0) checks

On lines;
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol/#L56-L57
Missing check for `newPauseGuardian`

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol/#L32
Missing check for `_initGovernor`

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol/#L115-L120
Missing check for `_newImplementation`

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol/#L69
Missing check for `_newAdmin`

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol/#L78
Missing check for `implementation`


## Missing check return of token transfers
There is no check on the return of the `transferFrom` method.
Lines:
- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol/#L235
- https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol/#L276

Try adding a require as a good practice, or use OZ safeERC20 libraries.
This is low for me because is always using the graph token that is a standard ERC20

## Avoid shadow variables
Variable `_admin` is shadowing function `_admin()` use a different name for this variable;

```diff
diff --git a/contracts/upgrades/GraphProxy.sol b/contracts/upgrades/GraphProxy.sol
index 69c88fd..d92bd30 100644
--- a/contracts/upgrades/GraphProxy.sol
+++ b/contracts/upgrades/GraphProxy.sol
@@ -43,7 +43,7 @@ contract GraphProxy is GraphProxyStorage {
      * @param _impl Address of the initial implementation
      * @param _admin Address of the proxy admin
      */
-    constructor(address _impl, address _admin) {
+    constructor(address _impl, address admin_) {
         assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));
         assert(
             IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)
@@ -53,7 +53,7 @@ contract GraphProxy is GraphProxyStorage {
                 bytes32(uint256(keccak256("eip1967.proxy.pendingImplementation")) - 1)
         );

-        _setAdmin(_admin);
+        _setAdmin(admin_);
         _setPendingImplementation(_impl);
     }
```


# NC

## Private variables should start with underscore

On [contracts/governance/Managed.sol/#L28](https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol/#L28)
So, `addressCache` should be named `_addressCache`