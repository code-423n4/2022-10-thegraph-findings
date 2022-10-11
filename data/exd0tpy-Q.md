## Low Risk
### Unspecific Compiler Version Pragma

BridgeEscrow.sol::3 => pragma solidity ^0.7.6;
Governed.sol::3 => pragma solidity ^0.7.6;
GraphGovernance.sol::3 => pragma solidity ^0.7.6;
GraphProxy.sol::3 => pragma solidity ^0.7.6;
GraphProxyStorage.sol::3 => pragma solidity ^0.7.6;
GraphTokenUpgradeable.sol::3 => pragma solidity ^0.7.6;
L1GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
L2GraphToken.sol::3 => pragma solidity ^0.7.6;
L2GraphTokenGateway.sol::3 => pragma solidity ^0.7.6;
Managed.sol::3 => pragma solidity ^0.7.6;
Pausable.sol::3 => pragma solidity ^0.7.6;

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

### Using transferFrom

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L235
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L276

ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard.
It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.
