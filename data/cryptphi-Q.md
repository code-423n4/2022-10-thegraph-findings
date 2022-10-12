
1. L2GraphTokenGateway.finalizeInboundTransfer() does not need to be payable since it checks for zero eth sent in the function call since the function does not allow eth to be sent due to the require check

2. L2GraphTokenGateway.outboundTransfer() does not need to be payable since it checks for zero eth sent in the function call since function does not allow eth to be sent due to the require check

3. Local variable shadowing
The following are occurrences of local variable shadwing. 
- GraphProxy.constructor._admin shadows the function GraphProxyStorage.admin()
- GraphProxy._acceptUpgrade._pendingImplementation  shadows the function GraphProxyStorage._pendingImplementation()
- Controller.setPartialPaused._partialPaused shadows Pausable._partialPaused() state vaiable variable
- Controller.setPaused._paused shadows Pausable._paused() state vaiable variable

Recommendation: Rename the local variables that shadow another component.


4. Missing zero address check
The following are missing checks for zero address to ensure the address state variable is not set to zero address.

**Occurrences**:
- L2GraphTokenGateway.initialize() - _controller argument
- L1GraphTokenGateway.initialize() - _controller argument
- BridgeEscrow.initialize() - _controller argument

Recommendation: apply necessary checks to ensure zero address is not supplied as input.

5. Use of floating pragma
The following contracts have a floating pragma set:
- BridgeEscrow.sol
- GraphUpgradeable.sol
- Governed.sol
- Pausable.sol
- L2GraphToken.sol
- GraphProxyAdmin.sol
- GraphProxyStorage.sol
- GraphProxy.sol
- Managed.sol
- GraphTokenUpgradeable.sol
- L2GraphTokenGateway.sol
- L1GraphTokenGateway.sol
- GraphTokenGateway.sol
- IGraphCurationToken.sol
- ICallhookReceiver.sol
- IGraphProxy.sol
- IEpochManager.sol
- IController.sol
- IGraphToken.sol
- IRewardsManager.sol
- IStakingData.sol
- ICuration.sol
- IStaking.sol
