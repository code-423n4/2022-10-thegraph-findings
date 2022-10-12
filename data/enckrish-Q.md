# Low severity issues

## [L-1] Use of floating pragma
Solidity compiler version should be locked to the version tested with. See https://swcregistry.io/docs/SWC-103.

## [L-2] Missing zero-address checks in GraphProxy.constructor

## [L-3] Should check whether minter already exists
GraphTokenUpgradeable.addMinter doesn't check if the address is already a minter and due to that. Anything that relies on `MinterAdded` event, will be affected. 

# Non-critical issues

## [N-1] Use of same name for local and contract-level elements
`GraphProxy._acceptUpgrade._pendingImplementation` shadows a function by the same name. It is not recommended to use same name for both. In cases where we can pass a function pointer, this would have made the code much less readable.

## [N-2] Redundant zero-address check in `GraphProxy._acceptUpgrade`
`_pendingImplementation` is already checked for whether it is a contract, by checking `extcodesize`. For the zero-address, `extcodesize` is zero, so it won't pass the `isContract` check and so needn't be checked again.

## [N-3] Revert statements follow different styles
The revert reason in most of the contracts follow the style: "revert reason", while in the L#GraphTokenGateway contracts, it is "REVERT_REASON". Consider following the same style.

## [N-4] Missing events in BridgeEscrow
BridgeEscrow.approveAll and BridgeEscrow.revokeAll perform state changes critical to the bridge's operation and should emit events.

## [N-5] Should check the return value of token `approve` and `decreaseAllowance` in BridgeEscrow

## [N-6] Constructor visibility is ignored in Solidity >= 0.7 and can be removed

## [N-7] Unnecessary variable in L2GraphTokenGateway.finalizeInboundTransfer
In the code:
```sol
bytes memory callhookData;
{
    bytes memory gatewayData;
    (gatewayData, callhookData) = abi.decode(_data, (bytes, bytes));
}
```
`callhookData` is not used anywhere, and can be removed.