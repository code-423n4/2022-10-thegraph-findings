## Use Custom Errors Instead of Strings for Revert and Require

Require strings and revert strings use about 50 more gas than custom errors.

***

require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphUpgradeable.sol#L24

require(msg.sender == _implementation(), "Caller must be the implementation");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphUpgradeable.sol#L32

require(msg.sender == governor, "Only Governor can call");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Governed.sol#L24

require(_newGovernor != address(0), "Governor must be set");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Governed.sol#L41

require(msg.sender == gateway, "NOT_GATEWAY");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L36

require(_owner != address(0), "Owner must be set");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L49

require(_gw != address(0), "INVALID_GATEWAY");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L60

require(_addr != address(0), "INVALID_L1_ADDRESS");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/L2GraphToken.sol#L70

require(msg.sender == _admin(), "Caller must be admin");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxyStorage.sol#L62

require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L105

require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L141

require(msg.sender != _admin(), "Cannot fallback to proxy target");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L157

require(!controller.paused(), "Paused");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L44

require(!controller.partialPaused(), "Partial-paused");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L45

require(!controller.paused(), "Paused");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L49

require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L53

require(msg.sender == address(controller), "Caller must be Controller");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L57

require(_controller != address(0), "Controller must be set");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L104

require(isMinter(msg.sender), "Only minter can call");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L60

require(_owner == recoveredAddress, "GRT: invalid permit");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L94

require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L95

require(_account != address(0), "INVALID_MINTER");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L106

require(_minters[_account], "NOT_A_MINTER");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L115

require(_minters[msg.sender], "NOT_A_MINTER");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/token/GraphTokenUpgradeable.sol#L123

require(_l2Router != address(0), "INVALID_L2_ROUTER");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L98

require(_l1GRT != address(0), "INVALID_L1_GRT");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L108

require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L118

require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L145

require(_amount > 0, "INVALID_ZERO_AMOUNT");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L146

require(msg.value == 0, "INVALID_NONZERO_VALUE");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L147

require(_to != address(0), "INVALID_DESTINATION");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L148

require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L153

require(_l1Token == l1GRT, "TOKEN_NOT_GRT");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L233

require(msg.value == 0, "INVALID_NONZERO_VALUE");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L234

require(inbox != address(0), "INBOX_NOT_SET");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L74

require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L78

require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L82

require(_inbox != address(0), "INVALID_INBOX");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L110

require(_l1Router != address(0), "INVALID_L1_ROUTER");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L111

require(_l2GRT != address(0), "INVALID_L2_GRT");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L122

require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L132

require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L142

require(_newWhitelisted != address(0), "INVALID_ADDRESS");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L153

require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L154

require(_notWhitelisted != address(0), "INVALID_ADDRESS");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L165

require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L166

require(_l1Token == address(token), "TOKEN_NOT_GRT");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L200

require(_amount > 0, "INVALID_ZERO_AMOUNT");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L201

require(_to != address(0), "INVALID_DESTINATION");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L202

require(
    extraData.length == 0 || callhookWhitelist[msg.sender] == true,
    "CALL_HOOK_DATA_NOT_ALLOWED"
);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L213-L216

require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L217

require(msg.value >= expectedEth, "WRONG_ETH_VALUE");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L224

require(_l1Token == address(token), "TOKEN_NOT_GRT");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L271

require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L275

require(_newPauseGuardian != address(0), "PauseGuardian must be set");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L31

require(!_paused, "Paused (contract)");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/GraphTokenGateway.sol#L40

## Limit Message Strings for Revert and Require to 32 Bytes

Require strings and revert strings longer than 32 bytes need MSTORE (3 gas).
Try to use shorter descriptions. Custom errors would be even better (see above).

***

require(msg.sender == _implementation(), "Caller must be the implementation");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphUpgradeable.sol#L32

require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L105

require(Address.isContract(_pendingImplementation), "Implementation must be a contract");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L141

require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Managed.sol#L53

## Require Statements with Multiple Checks Need More Gas than Multiple Require Statements.

See link for a detailed discussion.
https://github.com/code-423n4/2022-01-xdefi-findings/issues/128

***

require(
    pendingGovernor != address(0) && msg.sender == pendingGovernor,
    "Caller must be pending governor"
);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/governance/Governed.sol#L54-L57

require(
    _pendingImplementation != address(0) && msg.sender == _pendingImplementation,
    "Caller must be the pending implementation"
);
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/upgrades/GraphProxy.sol#L142-L145

require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
https://github.com/code-423n4/2022-10-thegraph/tree/main/contracts/gateway/L1GraphTokenGateway.sol#L142


