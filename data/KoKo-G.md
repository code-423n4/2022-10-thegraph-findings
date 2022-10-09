# CONSTANTS SHOULD BE MARKED AS `IMMUTABLE` OR `CONSTANT`

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L29

# DO NOT USE BOOLEANS, USE UINTS INSTEAD (0 AND 1 FOR EXAMPLE)

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L8 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L10

# DON’T COMPARE BOOLEAN EXPRESSIONS TO BOOLEAN LITERALS

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L214

# STRING LONGER THAN 32 BYTES INCUR OVERHEAD

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L105 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L142-L145

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L32

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L19-L22

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L53

# PACK STORAGE VARIABLES IN FEWER SLOTS

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L8

# STATE VARIABLES SHOULD BE CACHED IN STACK VARIABLES RATHER THAN RE-READING THEM FROM STORAGE

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L55-L66

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L27-L34 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L41-L48

# FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L80 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L90

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L40

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L52 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L56

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L50

# USE MULTIPLE REQUIRE STATEMENTS INSTEAD OF ONE TO SAVE GAS (`&&`)

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L142-L145

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L54-L57

# <X < Y + 1> is cheaper than <X <= Y>

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L95

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L224 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L275

# USE CUSTOM ERRORS RATHER THAN REVERT()/REQUIRE() STRINGS TO SAVE DEPLOYMENT GAS

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L105 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L141 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L157

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L60 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L94 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L95 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L115

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L69-L72 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L108 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L118 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L145 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L146 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L148 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L153 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L233

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L24 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L54-L57

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/arbitrum/L1ArbitrumMessenger.sol#L100

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L78 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L82 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L110 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L122 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L132 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L142 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L153 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L166 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L201 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L202 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L213-L216 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L217 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L224 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L275

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L62

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L36 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L49 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L70

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L44 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L49 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L53 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L104

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L24 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol#L32

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L19-L22 \
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L40

# INLINE FUNCTION WHERE POSSIBLE

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L286

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L290

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L195
