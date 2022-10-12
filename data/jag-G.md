**[G-01] Use local variable instead of storage variable for event emit**
```diff
diff --git a/contracts/governance/Governed.sol b/contracts/governance/Governed.sol
index 2a87389..266abd2 100644
--- a/contracts/governance/Governed.sol
+++ b/contracts/governance/Governed.sol
@@ -43,7 +43,7 @@ contract Governed {
         address oldPendingGovernor = pendingGovernor;
         pendingGovernor = _newGovernor;
 
-        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
+        emit NewPendingOwnership(oldPendingGovernor, _newGovernor);
     }
 
     /**
@@ -63,6 +63,6 @@ contract Governed {
         pendingGovernor = address(0);
 
         emit NewOwnership(oldGovernor, governor);
-        emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
+        emit NewPendingOwnership(oldPendingGovernor, address(0));
     }
 }
diff --git a/contracts/governance/Pausable.sol b/contracts/governance/Pausable.sol
index b1ecbdf..7cad232 100644
--- a/contracts/governance/Pausable.sol
+++ b/contracts/governance/Pausable.sol
@@ -55,6 +55,6 @@ contract Pausable {
     function _setPauseGuardian(address newPauseGuardian) internal {
         address oldPauseGuardian = pauseGuardian;
         pauseGuardian = newPauseGuardian;
-        emit NewPauseGuardian(oldPauseGuardian, pauseGuardian);
+        emit NewPauseGuardian(oldPauseGuardian, newPauseGuardian);
     }
 }
diff --git a/contracts/l2/token/L2GraphToken.sol b/contracts/l2/token/L2GraphToken.sol
index ec6ca4e..3f74faf 100644
--- a/contracts/l2/token/L2GraphToken.sol
+++ b/contracts/l2/token/L2GraphToken.sol
@@ -59,7 +59,7 @@ contract L2GraphToken is GraphTokenUpgradeable, IArbToken {
     function setGateway(address _gw) external onlyGovernor {
         require(_gw != address(0), "INVALID_GATEWAY");
         gateway = _gw;
-        emit GatewaySet(gateway);
+        emit GatewaySet(_gw);
     }
 
     /**
```

**[G-02] Move storage variables into fewer storage slots** 
bool and address can be packed into slot 0.
```diff
diff --git a/contracts/governance/Pausable.sol b/contracts/governance/Pausable.sol
index b1ecbdf..872a8d3 100644
--- a/contracts/governance/Pausable.sol
+++ b/contracts/governance/Pausable.sol
@@ -8,14 +8,12 @@ contract Pausable {
     bool internal _partialPaused;
     // Paused will pause all major protocol functions
     bool internal _paused;
-
+    // Pause guardian is a separate entity from the governor that can pause
+    address public pauseGuardian;
     // Time last paused for both pauses
     uint256 public lastPausePartialTime;
     uint256 public lastPauseTime;
 
-    // Pause guardian is a separate entity from the governor that can pause
-    address public pauseGuardian;
-
     event PartialPauseChanged(bool isPaused);
     event PauseChanged(bool isPaused);
     event NewPauseGuardian(address indexed oldPauseGuardian, address indexed pauseGuardian);
```
**[G-03] Use custom errors instead of require() and revert()**
require() instance : 110
revert() instance : 2
```
contracts/gateway/GraphTokenGateway.sol:19:        require(
contracts/gateway/GraphTokenGateway.sol:31:        require(_newPauseGuardian != address(0), "PauseGuardian must be set");
contracts/gateway/GraphTokenGateway.sol:40:        require(!_paused, "Paused (contract)");
contracts/gateway/L1GraphTokenGateway.sol:74:        require(inbox != address(0), "INBOX_NOT_SET");
contracts/gateway/L1GraphTokenGateway.sol:78:        require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");
contracts/gateway/L1GraphTokenGateway.sol:82:        require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");
contracts/gateway/L1GraphTokenGateway.sol:110:        require(_inbox != address(0), "INVALID_INBOX");
contracts/gateway/L1GraphTokenGateway.sol:111:        require(_l1Router != address(0), "INVALID_L1_ROUTER");
contracts/gateway/L1GraphTokenGateway.sol:122:        require(_l2GRT != address(0), "INVALID_L2_GRT");
contracts/gateway/L1GraphTokenGateway.sol:132:        require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");
contracts/gateway/L1GraphTokenGateway.sol:142:        require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
contracts/gateway/L1GraphTokenGateway.sol:153:        require(_newWhitelisted != address(0), "INVALID_ADDRESS");
contracts/gateway/L1GraphTokenGateway.sol:154:        require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
contracts/gateway/L1GraphTokenGateway.sol:165:        require(_notWhitelisted != address(0), "INVALID_ADDRESS");
contracts/gateway/L1GraphTokenGateway.sol:166:        require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
contracts/gateway/L1GraphTokenGateway.sol:200:        require(_l1Token == address(token), "TOKEN_NOT_GRT");
contracts/gateway/L1GraphTokenGateway.sol:201:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
contracts/gateway/L1GraphTokenGateway.sol:202:        require(_to != address(0), "INVALID_DESTINATION");
contracts/gateway/L1GraphTokenGateway.sol:213:                require(
contracts/gateway/L1GraphTokenGateway.sol:217:                require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
contracts/gateway/L1GraphTokenGateway.sol:224:                    require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
contracts/gateway/L1GraphTokenGateway.sol:271:        require(_l1Token == address(token), "TOKEN_NOT_GRT");
contracts/gateway/L1GraphTokenGateway.sol:275:        require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
contracts/upgrades/GraphProxy.sol:105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
contracts/upgrades/GraphProxy.sol:133:        require(success);
contracts/upgrades/GraphProxy.sol:141:        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
contracts/upgrades/GraphProxy.sol:142:        require(
contracts/upgrades/GraphProxy.sol:157:        require(msg.sender != _admin(), "Cannot fallback to proxy target");
contracts/upgrades/GraphProxyAdmin.sol:34:        require(success);
contracts/upgrades/GraphProxyAdmin.sol:47:        require(success);
contracts/upgrades/GraphProxyAdmin.sol:59:        require(success);
contracts/upgrades/GraphProxyStorage.sol:62:        require(msg.sender == _admin(), "Caller must be admin");
contracts/upgrades/GraphUpgradeable.sol:24:        require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
contracts/upgrades/GraphUpgradeable.sol:32:        require(msg.sender == _implementation(), "Caller must be the implementation");
contracts/governance/Controller.sol:34:        require(
contracts/governance/Controller.sol:60:        require(_contractAddress != address(0), "Contract address must be set");
contracts/governance/Controller.sol:88:        require(_controller != address(0), "Controller must be set");
contracts/governance/Controller.sol:115:        require(_newPauseGuardian != address(0), "PauseGuardian must be set");
contracts/governance/Governed.sol:24:        require(msg.sender == governor, "Only Governor can call");
contracts/governance/Governed.sol:41:        require(_newGovernor != address(0), "Governor must be set");
contracts/governance/Governed.sol:54:        require(
contracts/governance/GraphGovernance.sol:39:        require(_governor != address(0), "governor != 0");
contracts/governance/GraphGovernance.sol:69:        require(_proposalId != 0x0, "!proposalId");
contracts/governance/GraphGovernance.sol:70:        require(_votes != 0x0, "!votes");
contracts/governance/GraphGovernance.sol:71:        require(_resolution != ProposalResolution.Null, "!resolved");
contracts/governance/GraphGovernance.sol:72:        require(!isProposalCreated(_proposalId), "proposed");
contracts/governance/GraphGovernance.sol:98:        require(_proposalId != 0x0, "!proposalId");
contracts/governance/GraphGovernance.sol:99:        require(_votes != 0x0, "!votes");
contracts/governance/GraphGovernance.sol:100:        require(_resolution != ProposalResolution.Null, "!resolved");
contracts/governance/GraphGovernance.sol:101:        require(isProposalCreated(_proposalId), "!proposed");
contracts/governance/Managed.sol:44:        require(!controller.paused(), "Paused");
contracts/governance/Managed.sol:45:        require(!controller.partialPaused(), "Partial-paused");
contracts/governance/Managed.sol:49:        require(!controller.paused(), "Paused");
contracts/governance/Managed.sol:53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
contracts/governance/Managed.sol:57:        require(msg.sender == address(controller), "Caller must be Controller");
contracts/governance/Managed.sol:104:        require(_controller != address(0), "Controller must be set");
contracts/l2/token/GraphTokenUpgradeable.sol:60:        require(isMinter(msg.sender), "Only minter can call");
contracts/l2/token/GraphTokenUpgradeable.sol:94:        require(_owner == recoveredAddress, "GRT: invalid permit");
contracts/l2/token/GraphTokenUpgradeable.sol:95:        require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
contracts/l2/token/GraphTokenUpgradeable.sol:106:        require(_account != address(0), "INVALID_MINTER");
contracts/l2/token/GraphTokenUpgradeable.sol:115:        require(_minters[_account], "NOT_A_MINTER");
contracts/l2/token/GraphTokenUpgradeable.sol:123:        require(_minters[msg.sender], "NOT_A_MINTER");
contracts/l2/token/L2GraphToken.sol:36:        require(msg.sender == gateway, "NOT_GATEWAY");
contracts/l2/token/L2GraphToken.sol:49:        require(_owner != address(0), "Owner must be set");
contracts/l2/token/L2GraphToken.sol:60:        require(_gw != address(0), "INVALID_GATEWAY");
contracts/l2/token/L2GraphToken.sol:70:        require(_addr != address(0), "INVALID_L1_ADDRESS");
contracts/upgrades/GraphProxy.sol:105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
contracts/upgrades/GraphProxy.sol:133:        require(success);
contracts/upgrades/GraphProxy.sol:141:        require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
contracts/upgrades/GraphProxy.sol:142:        require(
contracts/upgrades/GraphProxy.sol:157:        require(msg.sender != _admin(), "Cannot fallback to proxy target");
contracts/upgrades/GraphProxyAdmin.sol:34:        require(success);
contracts/upgrades/GraphProxyAdmin.sol:47:        require(success);
contracts/upgrades/GraphProxyAdmin.sol:59:        require(success);
contracts/upgrades/GraphProxyStorage.sol:62:        require(msg.sender == _admin(), "Caller must be admin");
contracts/upgrades/GraphUpgradeable.sol:24:        require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
contracts/upgrades/GraphUpgradeable.sol:32:        require(msg.sender == _implementation(), "Caller must be the implementation");
contracts/governance/Controller.sol:34:        require(
contracts/governance/Controller.sol:60:        require(_contractAddress != address(0), "Contract address must be set");
contracts/governance/Controller.sol:88:        require(_controller != address(0), "Controller must be set");
contracts/governance/Controller.sol:115:        require(_newPauseGuardian != address(0), "PauseGuardian must be set");
contracts/governance/Governed.sol:24:        require(msg.sender == governor, "Only Governor can call");
contracts/governance/Governed.sol:41:        require(_newGovernor != address(0), "Governor must be set");
contracts/governance/Governed.sol:54:        require(
contracts/governance/GraphGovernance.sol:39:        require(_governor != address(0), "governor != 0");
contracts/governance/GraphGovernance.sol:69:        require(_proposalId != 0x0, "!proposalId");
contracts/governance/GraphGovernance.sol:70:        require(_votes != 0x0, "!votes");
contracts/governance/GraphGovernance.sol:71:        require(_resolution != ProposalResolution.Null, "!resolved");
contracts/governance/GraphGovernance.sol:72:        require(!isProposalCreated(_proposalId), "proposed");
contracts/governance/GraphGovernance.sol:98:        require(_proposalId != 0x0, "!proposalId");
contracts/governance/GraphGovernance.sol:99:        require(_votes != 0x0, "!votes");
contracts/governance/GraphGovernance.sol:100:        require(_resolution != ProposalResolution.Null, "!resolved");
contracts/governance/GraphGovernance.sol:101:        require(isProposalCreated(_proposalId), "!proposed");
contracts/governance/Managed.sol:44:        require(!controller.paused(), "Paused");
contracts/governance/Managed.sol:45:        require(!controller.partialPaused(), "Partial-paused");
contracts/governance/Managed.sol:49:        require(!controller.paused(), "Paused");
contracts/governance/Managed.sol:53:        require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
contracts/governance/Managed.sol:57:        require(msg.sender == address(controller), "Caller must be Controller");
contracts/governance/Managed.sol:104:        require(_controller != address(0), "Controller must be set");
contracts/l2/gateway/L2GraphTokenGateway.sol:69:        require(
contracts/l2/gateway/L2GraphTokenGateway.sol:98:        require(_l2Router != address(0), "INVALID_L2_ROUTER");
contracts/l2/gateway/L2GraphTokenGateway.sol:108:        require(_l1GRT != address(0), "INVALID_L1_GRT");
contracts/l2/gateway/L2GraphTokenGateway.sol:118:        require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
contracts/l2/gateway/L2GraphTokenGateway.sol:145:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
contracts/l2/gateway/L2GraphTokenGateway.sol:146:        require(_amount > 0, "INVALID_ZERO_AMOUNT");
contracts/l2/gateway/L2GraphTokenGateway.sol:147:        require(msg.value == 0, "INVALID_NONZERO_VALUE");
contracts/l2/gateway/L2GraphTokenGateway.sol:148:        require(_to != address(0), "INVALID_DESTINATION");
contracts/l2/gateway/L2GraphTokenGateway.sol:153:        require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
contracts/l2/gateway/L2GraphTokenGateway.sol:233:        require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
contracts/l2/gateway/L2GraphTokenGateway.sol:234:        require(msg.value == 0, "INVALID_NONZERO_VALUE");

contracts/upgrades/GraphProxy.sol:179:                revert(ptr, size)
contracts/upgrades/GraphProxy.sol:179:                revert(ptr, size)
```
**[G-04] require() strings longer than 32 bytes cost extra gas**
```
contracts/upgrades/GraphProxy.sol:105:        require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
```

**[G-05] Use a more recent version of solidity**
Solidity version used is 0.7.6, can be updated.