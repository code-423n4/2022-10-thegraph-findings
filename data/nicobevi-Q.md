# QA and LOWS

# BridgeEscrow.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## Unchecked address input

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol#L20-L22

### Problem

_controller could be address(0). We recommend to add a verification.

### Recommended Mitigations Steps:

Add a verification such as `require(_controller != address(0), "invalid _controller");`.

# GraphTokenGateway.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

# L1GraphTokenGateway.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## Unsafe safeTransfer call

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L276

### Problem

Some ERC20 tokens functions don’t return a boolean, for example USDT, BNB, OMG. So the VotingEscrow contract simply won’t work with tokens like that as the token.

### Recommended Mitigations Steps:

Use the OpenZepplin’s `safeTransferFrom` functions.

## Unchecked addresses params

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L263-L279

### Problem

Params `_from`, `_to` could be address(0) generating some unexpected behaviours.

### Recommended Mitigations Steps:

Add validations before the transfer is made.

# Governed.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## _initGovernor could be address(0)

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol#L32

### Problem

Param `_initGovernor` could be address(0) generating some unexpected behaviours.

### Recommended Mitigations Steps:

Add a validation in `_initialize()`

# Managed.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## Duplicated code on _notPartialPaused()

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L43-L46

### Problem

The line `require(!controller.paused(), "Paused");` is duplicated from what is defined on `_notPaused()`.

### Recommended Mitigations Steps:

Replace that line with a call to `_notPaused();`

## Keccak calculations could be saved as constants to save gas

### Lines:

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L114
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L122
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L130
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L138
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L146
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol#L154

### Problem

The keccak calculation is being made on every call wasting gas.

### Recommended Mitigations Steps:

Move those calculations as constansts and use them on the functions:

```solidity
  bytes32 private CURATION_HASH = keccak256("Curation");
  ....
  bytes32 private GRAPH_TOKEN_GATEWAY_HASH = keccak256("GraphTokenGateway");
```

# Pausable.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## newPauseGuardian could be address(0)

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol#L55-L59

### Problem

Param `newPauseGuardian` could be address(0) generating some unexpected behaviours.

### Recommended Mitigations Steps:

Add a validation in `_setPauseGuardian()`

# L2GraphTokenGateway.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## _controller could be address(0)

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#L87-L91

### Problem

Param `_controller` could be address(0) generating some unexpected behaviours.

### Recommended Mitigations Steps:

Add a validation in `initialize()`

# GraphTokenUpgradeable.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## Unnecesary use of SafeMath lib

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L7
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L97

### Problem

SafeMath.sol is being imported and used only on one place for a nonce increase. There is no overflow danger on this use case so we recommend to remove the lib.

### Recommended Mitigations Steps:

Remove SafeMath.sol and change the nonce increment line to 

```solidity
++nonces[_owner];
```

## `permit()` could be replaced using Openzeppelin's ERC20Permit.sol contract instead

### Lines:

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L74-L99

### Problem:

Reimplementation of functionality that it is already implemented on standard libraries

### Recommended Mitigations Steps:

Use `@openzeppelin-contracts/token/ERC20/extensions/draft-ERC20Permit.sol` and adapt the `permit()` function with custom functionality.

## `_deadline` validation should be made before the signature recover is made to save gas.

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L95

### Problem

A validation is being made after a ECDSA recover is being made. This calculation and gas effort is useless if that validation is not fullfilled.

### Recommended Mitigations Steps:

We recommend to move the validation above the ECDSA recover to save gas on those cases.

## `_owner` could be address(0) on initialization

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L150-L171

### Problem

`_owner` could be address(0) resulting on unexpected behaviours

### Recommended Mitigations Steps:

Add a validation and rejecting the transaction otherwise.

## `_account` could be address(0) on `_addMinter()` call

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L177-L180

### Problem

`_account` could be address(0) resulting on unexpected behaviours

### Recommended Mitigations Steps:

Add a validation and rejecting the transaction otherwise.

## `_account` could be address(0) on `_removeMinter()` call

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#L186-L189

### Problem

`_account` could be address(0) resulting on unexpected behaviours

### Recommended Mitigations Steps:

Add a validation and rejecting the transaction otherwise.

# L2GraphToken.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

# GraphProxy.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## Consider to extend `IGraphProxy` for consistency

### Lines 

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L16

### Problem

It's a good practice to extend Interfaces that it's supose that the contract is implementing

### Recommended Mitigations Steps:

Extends GraphProxy from IGraphProxy

## Shadowed variable `_admin` with `_admin()` function on constructor

### Lines:

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L46-L58

### Problem

Input variable `_admin` is shadowing function `_admin()` which is confusing and could generate errors.

### Recommended Mitigations Steps:

Change the name of the input variable.

## `admin()` visibility can be changed to view

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L69-L71

### Problem

`admin()` visibility could be changed to view since there is no storage modifications being made.

### Recommended Mitigations Steps:

Change visibility to view.

## `implementation()` visibility can be changed to view

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L82-L84

### Problem

`implementation()` visibility could be changed to view since there is no storage modifications being made.

### Recommended Mitigations Steps:

Change visibility to view.

## `pendingImplementation()` visibility can be changed to view

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L95-L97

### Problem

`pendingImplementation()` visibility could be changed to view since there is no storage modifications being made.

### Recommended Mitigations Steps:

Change visibility to view.

## Shadowed variable `_pendingImplementation` with `_pendingImplementation()` function on `acceptUpgrade()`

### Lines:

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#L139-L149

### Problem

Input variable `_pendingImplementation` is shadowing function `_pendingImplementation()` which is confusing and could generate errors.

### Recommended Mitigations Steps:

Change the name of the input variable.

# GraphProxyAdmin.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## Use a constant to improve readability

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L30-L36

### Problem

It is better for readability to save the hash as a constant and calculate it at constructon time

### Recommended Mitigations Steps:

Use a constant like

```solidity
bytes4 private constant IMPLEMENTATION_HASH = bytes4(keccak256("implementation()"));
```

## Missing error message

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L34

### Problem

It's recommended to add an error message for every require validation.

### Recommended Mitigations Steps:

Add an error message to the validation

## Use a constant to improve readability

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L43-L49

### Problem

It is better for readability to save the hash as a constant and calculate it at constructon time

### Recommended Mitigations Steps:

Use a constant like

```solidity
bytes4 private constant PENDING_IMPLEMENTATION_HASH = bytes4(keccak256("pendingImplementation()"));
```

## Missing error message

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L47

### Problem

It's recommended to add an error message for every require validation.

### Recommended Mitigations Steps:

Add an error message to the validation

## Use a constant to improve readability

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L55-L61

### Problem

It is better for readability to save the hash as a constant and calculate it at constructon time

### Recommended Mitigations Steps:

Use a constant like

```solidity
bytes4 private constant ADMIN_HASH = bytes4(keccak256("admin()"));
```

## Missing error message

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol#L59

### Problem

It's recommended to add an error message for every require validation.

### Recommended Mitigations Steps:

Add an error message to the validation.

## Unchecked address param

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L68

### Problem

Param `_newAdmin` could be address(0) generating some unexpected behaviours.

### Recommended Mitigations Steps:

Add validations before the transfer is made.

## Unchecked address param

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#L77

### Problem

Param `_implementation` could be address(0) generating some unexpected behaviours.

### Recommended Mitigations Steps:

Add validations before the transfer is made.

# GraphProxyStorage.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## Replace the hardcoded hashes with the actual keccak call

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L18
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L26
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L34

### Problem

It would be better to change the hardcoded hashes with the actual keccak256 calls to improve readability and to be sure that the hashes are correct.

### Recommended Mitigations Steps:

Replace the constants with

```solidity
  bytes32 internal constant IMPLEMENTATION_SLOT = keccak256("eip1967.proxy.implementation");
  bytes32 internal constant PENDING_IMPLEMENTATION_SLOT = keccak256("eip1967.proxy.pendingImplementation");
  bytes32 internal constant ADMIN_SLOT = keccak256("eip1967.proxy.admin");
```

## Unchecked address param

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L34

### Problem

Param `_newAdmin` could be address(0) generating some unexpected behaviours.

### Recommended Mitigations Steps:

Add validations before the transfer is made.

## Unchecked address param

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L115

### Problem

Param `_newImplementation` could be address(0) generating some unexpected behaviours.

### Recommended Mitigations Steps:

Add validations before the transfer is made.

## Unchecked address param

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol#L130

### Problem

Param `_newImplementation` could be address(0) generating some unexpected behaviours.

### Recommended Mitigations Steps:

Add validations before the transfer is made.

# GraphUpgradeable.sol

## Floating Pragma Version

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol/#L3

### Problem

Avoid to use a floating pragma version. It is always recommended that pragma should be fixed to the version that you are intending to deploy your contracts with. 

### Recommended Mitigations Steps:

Use a fixed pragma version.

## Use standar Openzeppelin's proxy contracts instead of implement it again

### Problem

It would be better to extend the contract with Openzeppelin's standar library instead of rewrite that logic

### Recommended Mitigations Steps:

Use Openzeppelin's proxy contracts.

## Replace the hardcoded hashes with the actual keccak call

### Lines

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol/#L17-L18

### Problem

It would be better to change the hardcoded hashes with the actual keccak256 calls to improve readability and to be sure that the hashes are correct.

### Recommended Mitigations Steps:

Replace the constants with

```solidity
  bytes32 internal constant IMPLEMENTATION_SLOT = keccak256("eip1967.proxy.implementation");
```