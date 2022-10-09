->USING > 0 COSTS MORE GAS THAN != 0 WHEN USED ON A UINT IN A REQUIRE() STATEMENT

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol#:~:text=require(_amount%20%3E%200%2C%20%22INVALID_ZERO_AMOUNT%22)%3B
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#:~:text=require(_amount%20%3E%200%2C%20%22INVALID_ZERO_AMOUNT%22)%3B

->SPLITTING REQUIRE() STATEMENTS THAT USE && SAVES GAS

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol#:~:text=a%20contract%22)%3B-,require(,)%3B,-_setImplementation(_pendingImplementation)%3B
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#:~:text=require(_escrow%20!%3D%20address(0)%20%26%26%20Address.isContract(_escrow)%2C%20%22INVALID_ESCROW%22)%3B


->USING BOOLS FOR STORAGE INCURS OVERHEAD

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol#:~:text=mapping(address%20%3D%3E%20bool)
https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol#:~:text=mapping(address%20%3D%3E%20bool)
