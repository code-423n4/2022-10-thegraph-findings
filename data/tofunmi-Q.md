## Contracts `BridgeEscrow`,`L2GraphToken`,`L1GraphTokenGateway`,and can always be reinitialized 
### Affected contracts and lines of code
- [contracts/gateway/BridgeEscrow.sol](contracts/gateway/BridgeEscrow.sol#L20) 
- [contracts/l2/token/L2GraphToken.sol](contracts/l2/token/L2GraphToken.sol#L48)
- [contracts/gateway/L1GraphTokenGateway.sol](contracts/gateway/L1GraphTokenGateway.sol#L99) 

### Recommended mitigation check
- Add a modifier check or a `bool` variable is stored in storage, and stored as `true` at the first initialization, and only initializes the contract, only when it is set to false
- If the intention is not to allow re-initialization every time, then a reinitialzation function should be implemented
```solidity
bool interanal initialized;

modifier initialized {
   if(initialized) revert();
}
```