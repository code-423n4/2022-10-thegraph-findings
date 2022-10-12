### [L-01] Unsafe `transferFrom` methods

Some token like USDT not return boolean value so recommend to use instead `safeTransferFrom` methods

File: c4udit/2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol 235, 276

```
token.transferFrom(from, escrow, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529fc4udit/2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol/#L235

```
token.transferFrom(escrow, _to, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529fc4udit/2022-10-thegraph/contracts/gateway/L1GraphTokenGateway.sol/#L276

### [L-02] Use specific compiler version

if contract is not library recomment use specific compiler version

File: c4udit/2022-10-thegraph/contracts/token/IGraphToken.sol 

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529fc4udit/2022-10-thegraph/contracts/token/IGraphToken.sol/#L3

File: c4udit/2022-10-thegraph/contracts/upgrades/GraphProxy.sol

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529fc4udit/2022-10-thegraph/contracts/upgrades/GraphProxy.sol/#L3

File: c4udit/2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol  

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529fc4udit/2022-10-thegraph/contracts/upgrades/GraphProxyAdmin.sol/#L  3

File: c4udit/2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol   3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529fc4udit/2022-10-thegraph/contracts/upgrades/GraphProxyStorage.sol/#L  3

File: c4udit/2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol   3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529fc4udit/2022-10-thegraph/contracts/upgrades/GraphUpgradeable.sol/#L  3

File: c4udit/2022-10-thegraph/contracts/upgrades/IGraphProxy.sol   3

```
pragma solidity ^0.7.6;
```

https://github.com/code-423n4/2022-10-thegraph/blob/fce4d7761db12f6f3edae9051cb54bf4ef11529fc4udit/2022-10-thegraph/contracts/upgrades/IGraphProxy.sol/#L  3

### [L-03] Use more recent compiler version