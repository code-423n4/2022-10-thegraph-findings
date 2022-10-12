
1 - Calculate the hash value and hardcode in the code could save gas
==

The hash calculations of each variable and put them in the code could save gas in the deployment

Instances of this issue:

```
contracts/l2/token/GraphTokenUpgradeable.sol#L34      bytes32 private constant DOMAIN_TYPE_HASH =
contracts/l2/token/GraphTokenUpgradeable.sol#L38      bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");
contracts/l2/token/GraphTokenUpgradeable.sol#L39      bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");
contracts/l2/token/GraphTokenUpgradeable.sol#L40      bytes32 private constant DOMAIN_SALT =
contracts/l2/token/GraphTokenUpgradeable.sol#L42      bytes32 private constant PERMIT_TYPEHASH =
```

**Links:**

[contracts/l2/token/GraphTokenUpgradeable.sol#L34-L45](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/l2/token/GraphTokenUpgradeable.sol#L34-L45)

2 - Remove unnecessary emit in order to save gas
==

The following emission is unnecessary and does not correspond to the function ```acceptOwnership```

```
contracts/governance/Governed.sol#L66                emit NewPendingOwnership(oldPendingGovernor, pendingGovernor);
```

**Links:**

[contracts/governance/Governed.sol#L66](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/governance/Governed.sol#L66)

3 - Use multiple require statements instead of &&
==

Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.

```
contracts/gateway/L1GraphTokenGateway.sol#L142      require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");
```

**Links:**

[contracts/gateway/L1GraphTokenGateway.sol#L142](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/L1GraphTokenGateway.sol#L142)
