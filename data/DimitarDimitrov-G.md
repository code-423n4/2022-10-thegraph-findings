# 1. There are many places that code use require. It's better to use revert with custom error. 
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L24
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L32
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L24
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L41
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L54-L57
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L36
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L49
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L60
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L70
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L34
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L47
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L59
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L62
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L105
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L133
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L141-L145
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L157
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L44-L57
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L104
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L145-L153
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L200-L202

This will save a lot of gas.

# 2. Immutable is more cheaper than constant.

# 3. Methods can be reduced
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L43-L80
Directly use method body inside modifier and remove methods.


