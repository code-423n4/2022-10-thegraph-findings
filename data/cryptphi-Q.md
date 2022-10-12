
1. L2GraphTokenGateway.finalizeInboundTransfer() does not need to be payable since it checks for zero eth sent in the function call since the function does not allow eth to be sent due to the require check

2. L2GraphTokenGateway.outboundTransfer() does not need to be payable since it checks for zero eth sent in the function call since function does not allow eth to be sent due to the require check

3. Local variable shadowing
GraphProxy.constructor._admin shadows the function GraphProxyStorage.admin()
GraphProxy._acceptUpgrade._pendingImplementation  shadows the function GraphProxyStorage._pendingImplementation()

Recommendation: Rename the local variables that shadow another component.


4. Missing zero address check
The following are missing checks for zero address to ensure the address state variable is not set to zero address.

**Occurrences**:
- L2GraphTokenGateway.initialize() - _controller argument
- L1GraphTokenGateway.initialize() - _controller argument
- BridgeEscrow.initialize() - _controller argument

Recommendation: apply necessary checks to ensure zero address is not supplied as input.