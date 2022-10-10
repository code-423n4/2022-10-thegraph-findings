  # Gas Optimizations

## Summary Of Findings:

|  | Issue 
-- | -- 
1 | Optimizations in `Governed.sol`
2 | Optimizations in `Pausable.sol`
3 | Optimizations in `L2GraphToken.sol`

## Detailed Report on Gas Optimization Findings:

### 1. <ins>Optimizations in `Governed.sol`</ins>
The optimizations are based on the following points:
- If the value which want to emit is already available in the function argument or in a memory variable, then avoid emitting it from a state variable.
- If a state variable is used more than once, cache it into a memory variable to save gas.
- Often the emit statement can be placed before the state variable is assignined to avoid reading from the state variable twice.


The following diff along with comments show the exact optimizations. 
```diff
diff --git "a/.\\orig.sol" "b/.\\modified.sol"
index 12eced1..b8b78f6 100644
--- "a/.\\orig.sol"
+++ "b/.\\modified.sol"
@@ -43,7 +43,7 @@ contract Governed {
         address oldPendingGovernor = pendingGovernor;
         pendingGovernor = _newGovernor;
 
-        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
+        emit NewPendingOwnership(oldPendingGovernor, _newGovernor);   //@audit gas saving: emit _newGovernor which was just assigend to pendingGovernor
     }
 
     /**
@@ -51,18 +51,16 @@ contract Governed {
      * This function must called by the pending governor.
      */
     function acceptOwnership() external {
+        address oldPendingGovernor = pendingGovernor;  //@audit gas saving: cache it. read twice within require statement
         require(
-            pendingGovernor != address(0) && msg.sender == pendingGovernor,
+            oldPendingGovernor != address(0) && msg.sender == oldPendingGovernor,
             "Caller must be pending governor"
         );
-
         address oldGovernor = governor;
-        address oldPendingGovernor = pendingGovernor;
-
-        governor = pendingGovernor;
+        governor = msg.sender;  //@audit gas saving: works because of previous require statement
         pendingGovernor = address(0);
-
-        emit NewOwnership(oldGovernor, governor);
-        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
+        emit NewOwnership(oldGovernor, msg.sender);  //@audit gas saving: emit cached value
+        emit NewPendingOwnership(msg.sender, address(0));  //@audit gas saving: works because of previous require statement
+        
     }
-}
\ No newline at end of file
+}
```

[Two functions](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L40-L67), `transferOwnership` and `acceptOwnership` were gas optimized. The hardhat gas results before and after optimization are given below:

 Function  | Avg gas cost before Optimization | Avg gas cost After Optimization | Gas saving
-- | -- | -- | -- 
transferOwnership | 47567  | 47561 | 6
acceptOwnership |  29993   | 29746  | 247

Total gas saved in `Governed.sol` = 6 + 247 = 253.

### 2. <ins>Optimizations in `Pausable.sol`</ins>
The optimizations are based on the following points:
- Sometimes storage slots could be saved by packing your state variables together to fit into one slot. This works only on variables which declared one after each other. 
- If the value which want to emit is already available in the function argument or in a memory variable, then avoid emitting it from a state variable.
- If a state variable is used more than once, cache it into a memory variable to save gas.


The following diff along with comments show the exact optimizations. 
```diff
diff --git "a/.\\orig.sol" "b/.\\modified.sol"
index 1f88ceb..e1b39a3 100644
--- "a/.\\orig.sol"
+++ "b/.\\modified.sol"
@@ -3,11 +3,6 @@
 pragma solidity ^0.7.6;
 
 contract Pausable {
-    // Partial paused paused exit and enter functions for GRT, but not internal
-    // functions, such as allocating
-    bool internal _partialPaused;
-    // Paused will pause all major protocol functions
-    bool internal _paused;
 
     // Time last paused for both pauses
     uint256 public lastPausePartialTime;
@@ -16,6 +11,13 @@ contract Pausable {
     // Pause guardian is a separate entity from the governor that can pause
     address public pauseGuardian;
 
+    //@audit gas saving: moved the two bool's here to save one storage slot.
+    // Partial paused paused exit and enter functions for GRT, but not internal
+    // functions, such as allocating
+    bool internal _partialPaused;
+    // Paused will pause all major protocol functions
+    bool internal _paused;
+
     event PartialPauseChanged(bool isPaused);
     event PauseChanged(bool isPaused);
     event NewPauseGuardian(address indexed oldPauseGuardian, address indexed pauseGuardian);
@@ -28,10 +30,10 @@ contract Pausable {
             return;
         }
         _partialPaused = _toPause;
-        if (_partialPaused) {
+        if (_toPause) {    //@audit gas saving: use _toPause as it was just assigned to _partialPaused
             lastPausePartialTime = block.timestamp;
         }
-        emit PartialPauseChanged(_partialPaused);
+        emit PartialPauseChanged(_toPause);   //@audit gas saving: use _toPause as it was just assigned to _partialPaused
     }
 
     /**
@@ -42,10 +44,10 @@ contract Pausable {
             return;
         }
         _paused = _toPause;
-        if (_paused) {
+        if (_toPause) {  //@audit gas saving: use _toPause as it was just assigned to _paused
             lastPauseTime = block.timestamp;
         }
-        emit PauseChanged(_paused);
+        emit PauseChanged(_toPause);  //@audit gas saving: use _toPause as it was just assigned to _paused
     }
 
     /**
@@ -55,6 +57,6 @@ contract Pausable {
     function _setPauseGuardian(address newPauseGuardian) internal {
         address oldPauseGuardian = pauseGuardian;
         pauseGuardian = newPauseGuardian;
-        emit NewPauseGuardian(oldPauseGuardian, pauseGuardian);
+        emit NewPauseGuardian(oldPauseGuardian, newPauseGuardian);  //@audit gas saving: emit newPauseGuardian which was just assigend to pauseGuardian
     }
-}
\ No newline at end of file
+}
```
[Three functions](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Pausable.sol#L26-L59), `_setPartialPaused` , `_setPaused` and `_setPauseGuardian` were gas optimized. The hardhat gas results before and after optimization are given below:


 Function  | Avg gas cost before Optimization | Avg gas cost After Optimization | Gas saving
-- | -- | -- | -- 
_setPartialPaused | 49195    | 45795   | 3400
_setPaused |  30817     | 29626    | 1191
_setPauseGuardian |  47575     | 47569  | 6

Total gas saved in `Pausable.sol` = 3400 + 1191 + 6 = 4597.

### 3. <ins>Optimizations in `L2GraphToken.sol`</ins>
In `L2GraphToken`, a function called [setGateway](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/L2GraphToken.sol#L59-L63) was optimized by emitting the function argument instead of the state variable which was just assigned.

The following diff along with comments show the exact optimizations. 
```diff
diff --git "a/.\\orig.sol" "b/.\\modified.sol"
index b3f01c6..374bcfd 100644
--- "a/.\\orig.sol"
+++ "b/.\\modified.sol"
@@ -59,7 +59,7 @@ contract L2GraphToken is GraphTokenUpgradeable, IArbToken {
     function setGateway(address _gw) external onlyGovernor {
         require(_gw != address(0), "INVALID_GATEWAY");
         gateway = _gw;
-        emit GatewaySet(gateway);
+        emit GatewaySet(_gw);  //@audit gas saving: use _gw as it was just assigned to gateway
     }
 
     /**
@@ -91,4 +91,4 @@ contract L2GraphToken is GraphTokenUpgradeable, IArbToken {
         burnFrom(_account, _amount);
         emit BridgeBurned(_account, _amount);
     }
-}
\ No newline at end of file
+}
```
The hardhat gas results before and after optimization are given below:

 Function  | Avg gas cost before Optimization | Avg gas cost After Optimization | Gas saving
-- | -- | -- | --  
setGateway | 54314      | 54296     | 18

Total gas saved in `L2GraphToken.sol` = 18.

## Conclusions:
  | Issue | Avg Gas Saved
-- | -- | -- 
1 | Optimizations in `Governed.sol` | 253
2 | Optimizations in `Pausable.sol` | 4597
3 | Optimizations in `L2GraphToken.sol` | 18

### TOTAL GAS SAVED = 253 + 4597 + 18 = <ins>4868</ins>.
