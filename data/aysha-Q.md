Unlocked pragma
Contracts should be deployed using the same compiler version/flags with which they have been tested. Locking the pragma (for e.g. by not using ^ in pragma solidity 0.5.10) ensures that contracts do not accidentally get deployed using an older compiler version with unfixed bugs.
https://swcregistry.io/docs/SWC-103
==========================================================

Solidity versions
Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs. https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity
==========================================================

Multiple Solidity pragma
It is better to use one Solidity compiler version across all contracts instead of different versions with different bugs and security checks.
https://github.com/crytic/slither/wiki/Detector-Documentation#different-pragma-directives-are-used
==========================================================

Two-step change of privileged roles
When privileged roles are being changed, it is recommended to follow a two-step approach: 1) The current privileged role proposes a new address for the change 2) The newly proposed address then claims the privileged role in a separate transaction. This two-step change allows accidental proposals to be corrected instead of leaving the system operationally with no/malicious privileged role. For e.g., in a single-step change, if the current admin accidentally changes the new admin to a zero-address or an incorrect address (where the private keys are not available), the system is left without an operational admin and will have to be redeployed.
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxy.sol#L104
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyAdmin.sol#L69
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/upgrades/GraphProxyStorage.sol#L80
==========================================================

ERC20 approve() race condition
Use safeIncreaseAllowance() and safeDecreaseAllowance() from OpenZeppelin’s SafeERC20 implementation to prevent race conditions from manipulating the allowance amounts.
https://swcregistry.io/docs/SWC-114

https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L29
==========================================================

Missing zero address validation
Setters of address type parameters should include a zero-address check otherwise contract functionality may become inaccessible or tokens burnt forever. (see here)
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L20
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L28
https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L36
==========================================================