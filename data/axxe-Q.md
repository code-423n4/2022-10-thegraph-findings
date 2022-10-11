## Old Solidity compiler version

It is a best practice to use a recent version of Solidity.
(For example, [BridgeEscrow](https://github.com/code-423n4/2022-10-thegraph/blob/309a188f7215fa42c745b136357702400f91b4ff/contracts/gateway/BridgeEscrow.sol#L3))

## `@dev` annotation for `external`/`public` functions

The `@notice` annotation is used to explain to an end user what an `external`/`public` function does.
But most of public functions (in the repo) are using the `@dev` annotation.

For example, in BridgeEscrow, `initialize`, `approveAll`, and `revokeAll` are `external` but the `@dev` annotation is used.

## BridgeEscrow : Missing address check

- `initialize()` => `_controller` not checked.
- `approveAll()` => `approveAll` not checked.
- `revokeAll()` => `_spender` not checked.

## Governed : Missing address check

- `_initialize()` => `_initGovernor` not checked.

## GraphUpgradeable : Should be `abstract` 

"This contract is intended to be inherited from upgradeable contracts."

## BridgeEscrow : Empty `IMPLEMENTATION_SLOT`

The `IMPLEMENTATION_SLOT` can't be initialized with the implementation address.
Therefore `initialize` can't be called.

I don't see any tests where we initialize the BridgeEscrow.

## Governed : Double event

In `acceptOwnership()` this is not necessary to emit `NewPendingOwnership` with `pendingGovernor = address(0)`.
`acceptOwnership` alone is enough to follow the ownership activity.