## 1. Use `Custom Error` instead of `require`
Almost in every contract, `require` is used. `Custom Errors` are a more gas-efficient approach.
Changes needs to be make in these files:
```
- contracts/upgrades/GraphUpgradeable.sol
- contracts/governance/Governed.sol
- contracts/l2/token/L2GraphToken.sol
- contracts/upgrades/GraphProxyAdmin.sol
- contracts/upgrades/GraphProxyStorage.sol
- contracts/upgrades/GraphProxy.sol
- contracts/governance/Managed.sol
- contracts/l2/token/GraphTokenUpgradeable.sol
- contracts/l2/gateway/L2GraphTokenGateway.sol
- contracts/gateway/L1GraphTokenGateway.sol
- contracts/gateway/GraphTokenGateway.sol
```

## 2. Use at least `0.8.4` or higher compiler version
`0.8.4` or higher compiler versions provide inbuilt gas optimizations :)