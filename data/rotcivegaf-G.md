
# Gas report

| G-N    |Issue|
|:------:|:----|
| [G&#x2011;01] | Use function parameter to avoid `MLOAD` |
| [G&#x2011;02] | Unused SafeMath.sol |
| [G&#x2011;03] | Use function saved var instead of `SLOAD` and `address(0)` |
| [G&#x2011;04] | Overcheck slots |
| [G&#x2011;05] | Getters with ownership modifier and marked as external |
| [G&#x2011;06] | Overcheck `_pendingImplementation` and `pendingGovernor` |

## [G-01] Use function parameter to avoid `MLOAD`

```solidity
File: /contracts/governance/Governed.sol

45        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
```

```solidity
File: /contracts/governance/Pausable.sol

31        if (_partialPaused) {

34        emit PartialPauseChanged(_partialPaused);

44        _paused = _toPause;

48        emit PauseChanged(_paused);

58        emit NewPauseGuardian(oldPauseGuardian, pauseGuardian);
```

```solidity
File: /contracts/l2/token/L2GraphToken.sol

62        emit GatewaySet(gateway);
```

## [G-02] Unused SafeMath.sol

```solidity
File: /contracts/l2/token/L2GraphToken.sol

 5 import "@openzeppelin/contracts/math/SafeMath.sol";

16    using SafeMath for uint256;
```

```solidity
File: /contracts/l2/gateway/L2GraphTokenGateway.sol

 7 import "@openzeppelin/contracts/math/SafeMath.sol";

24    using SafeMath for uint256;
```

```solidity
File: /contracts/l2/token/GraphTokenUpgradeable.sol

 7 import "@openzeppelin/contracts/math/SafeMath.sol";

29    using SafeMath for uint256;

From:
97        nonces[_owner] = nonces[_owner].add(1);
To:
97        ++nonces[_owner];
```

## [G-03] Use function saved var instead of `SLOAD` and `address(0)`

```diff
File: /contracts/governance/Governed.sol#L53-L67

     function acceptOwnership() external {
+        address oldPendingGovernor = pendingGovernor;
         require(
-            pendingGovernor != address(0) && msg.sender == pendingGovernor,
+            oldPendingGovernor != address(0) && msg.sender == oldPendingGovernor,
             "Caller must be pending governor"
         );

         address oldGovernor = governor;
-        address oldPendingGovernor = pendingGovernor;

-        governor = pendingGovernor;
+        governor = oldPendingGovernor;
         pendingGovernor = address(0);

-        emit NewOwnership(oldGovernor, governor);
-        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
+        emit NewOwnership(oldGovernor, oldPendingGovernor);
+        emit NewPendingOwnership(oldPendingGovernor, address(0));
     }
 }
```

## [G-04] Overcheck slots

Can check this slots in a test or you can calculate directly in the slot definition

```solidity
File: /contracts/upgrades/GraphProxy.sol

47        assert(ADMIN_SLOT == bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1));

48        assert(
49            IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)
50        );

51        assert(
52            PENDING_IMPLEMENTATION_SLOT ==
53                bytes32(uint256(keccak256("eip1967.proxy.pendingImplementation")) - 1)
54        );
```

## [G-05] Getters with ownership modifier and marked as external

These functions could be marked as view functions and the `ifAdminOrPendingImpl` modifier removed

```solidity
File: /contracts/upgrades/GraphProxy.sol

69    function admin() external ifAdminOrPendingImpl returns (address) {

82    function implementation() external ifAdminOrPendingImpl returns (address) {

95    function pendingImplementation() external ifAdminOrPendingImpl returns (address) {
```

## [G-06] Overcheck `_pendingImplementation` and `pendingGovernor`

If the `msg.sender` it's the `_pendingImplementation` => the `_pendingImplementation` it's not the `address(0)`
The `msg.sender == _pendingImplementation` check it's enough

```solidity
File: /contracts/upgrades/GraphProxy.sol

143            _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
```

The same case to `pendingGovernor`

```solidity
File: /governance/Governed.sol

55            pendingGovernor != address(0) && msg.sender == pendingGovernor,
```
