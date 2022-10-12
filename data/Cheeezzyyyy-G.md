### USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE() STRINGS TO SAVE GAS

Custom Errors can be implemented as such:

``INBOX_NOT_SET(); 
  NOT_FROM_BRIDGE();
  ONLY_COUNTERPART_GATEWAY(); 
INVALID_INBOX();
  INVALID_L1_ROUTER();
INVALID_L2_GRT();
INVALID_ESCROW();
INVALID_ADDRESS();
ALREADY_WHITELISTED();
NOT_WHITELISTED();
TOKEN_NOT_GRT():
INVALID_ZERO_AMOUNT();
INVALID_DESTINATION();
CALL_HOOK_DATA_NOT_ALLOWED();
TOKEN_NOT_GRT();
BRIDGE_OUT_OF_FUNDS();
``

While REVERT() statements can be also used to replace REQUIRE() statements like this i.e.:

[gateway/L1GraphTokenGateway.sol](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol)

- Use ``if(inbox == address(0)) revert INBOX_NOT_SET();`` instead of ``require(inbox != address(0), "INBOX_NOT_SET");``
- Use ``if(msg.sender != address(bridge)) revert NOT_FROM_BRIDGE();`` instead of ``require(inbox != address(0), "INBOX_NOT_SET");``
- Use ``if(l2ToL1Sender != l2Counterpart) revert ONLY_COUNTERPART_GATEWAY();`` instead of ``require(inbox != address(0), "INBOX_NOT_SET");``

### MORE REQUIRE STATEMENTS INSTEAD OF ‘&&’

Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.

Instances Found:

- [gateway/L1GraphTokenGateway.sol#L142](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142) Remedy: Use ``require(_escrow != address(0), "INVALID_ESCROW");  require(Address.isContract(_escrow), "INVALID_ESCROW");``  instead of ``require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");``


